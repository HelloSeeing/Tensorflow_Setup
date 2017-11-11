# 流程梳理

## 整体流程

```flow
st=>start: Start
te=>end: End
input_data=>inputoutput: Input 
--input_image image data in type of np.array
optional input--region_params
img_cond=>condition: im_width > 400 
and im_height > 400?
img_error=>operation: error, image size should be at least 400x400,
and denote this message by
--predict_msg
region_cond=>condition: exist region_params?
patch_method=>operation: use_region_flag = 0
region_method=>operation: use_region_flag = 1
pre_process=>subroutine: pre_process(input_image, use_region_flag, region_params)
pre_out=>inputoutput: pre_out
image_data_list, skin_ind, pre_msg
pre_cond=>condition: is pre_msg an error message?
main_input=>inputoutput: --main_img_data
images list in image_data_list that are of skin_ind
--num_top
how many probs that main_process will return
main_process=>subroutine: main_process(main_img_data, use_region_flag, num_top)
main_out=>inputoutput: main_out
full_prob, full_ind, dis_top_prob, dis_top_class, healthy_index, dis_index, main_msg
healthy_cond=>condition: healthy prob in full_prob > 0.95?
healthy_op=>operation: find all healthy patches in main_img_data by healthy index, gather as post_img_data
disease_op=>operation: find all disease patches in main_img_data by disease index, gather as post_img_data
post_input=>inputoutput: post_input
--post_img_data
--full_prob
--dis_top_class
--dis_top_prob
post_process=>subroutine: postprocess(post_img_data, if_healthy, full_prob, dis_top_class, dis_top_prob)
post_out=>inputoutput: post_out
predict_msg, predict_feature_dict, predict_merged_result
res_h_input=>inputoutput: Result Handler Input
--predict_msg
optional input--predict_feature_dict, default is None
optional input--predict_merged_result, default is None
result_handler=>subroutine: result_handler(predict_msg, predict_feature_dict, predict_merged_result)
resh_out=>inputoutput: Result Handler Output
--final_msg
--final_degree_if_confirm
--final_feature_dict
--final_merged_result
print_op=>operation: print final result
st->input_data->img_cond
region_cond(no)->patch_method->pre_process->pre_out->pre_cond
region_cond(yes)->region_method->pre_process->pre_out->pre_cond
img_cond(no)->img_error
img_cond(yes)->region_cond
pre_cond(no)->main_input->main_process->main_out->healthy_cond
pre_cond(yes)->error
healthy_cond(yes)->healthy_op->post_input
healthy_cond(no)->disease_op->post_input
post_input->post_process->post_out->res_h_input
img_error->res_h_input
res_h_input->result_handler->resh_out->print_op->te
```

## 预处理流程

```flow
pre_st=>start: pre_process start
pre_te=>end: pre_process end
input=>inputoutput: Input
--input_image, 
--use_region_flag, 
optional input--region_params
region_cond=>condition: use_region_flag = 1?
do_patch=>operation: slice input_image into patch list [patches]:
patch size is 256x256, 
slicing stepsize is 128
patch_op1=>operation: filter all patch in 'patches' by random forest, 
get classification result as 'predicts'
patch_op2=>operation: find skin patches, 
then record their indices as pre_out_skin_ind
patch_op3=>operation: pre_out_img_data = [patches]
do_crop=>operation: crop image_data by region_params
crop_op1=>operation: convert image_data to a list,
that is, pre_out_img_data = [image_data]
crop_op2=>operation: set indices of skin image,
that is, pre_out_skin_ind = [0]
pre_output=>inputoutput: Output 
--pre_msg if length of skin_ind is 0, this pre_msg is 'ERROR'
  else, pre_msg is 'INFO'
--pre_out_img_data, 
--pre_out_skin_ind
pre_st->input->region_cond
region_cond(no)->do_patch->patch_op1->patch_op2->patch_op3->pre_output->pre_te
region_cond(yes)->do_crop->crop_op1->crop_op2->pre_output->pre_te
```

## 中处理流程

```flow
main_start=>start: main_start
main_end=>end: main_end
main_input=>inputoutput: Input
--image_data_list, 
--use_region_flag, 
--num_top
region_cond=>condition: use_region_flag = 1?
patch_method=>operation: *Patches Method*
filter each element in image_data_list by cnn_patches_filter
region_method=>operation: *Region Method*
filter each element in image_data_list by cnn_region_filter
cnn_output=>inputoutput: cnn_output
--img_probs probabilities of each element 
res_handle=>operation: accumulate all img_probs to one normalized prob vector, and get
--full_prob this prob vector
--dis_prob delete prob of class healthy in full_prob, then normalized
main_output=>inputoutput: Output
--full_prob
--full_ind index list of descending ordered full_prob
--dis_top_prob the first num_top probabilities of descending ordered dis_prob
--dis_top_class indices of dis_top_prob in dis_prob
--healthy_index indices of image patches which are not belong to class healthy
--dis_index indices of image patches which are not belong to class healthy
--main_msg main message, always is 'INFO'
main_start->main_input->region_cond
region_cond(yes)->region_method->cnn_output
region_cond(no)->patch_method->cnn_output
cnn_output->res_handle->main_output->main_end
```

## 后处理流程

```flow
post_start=>start: post_start
post_input=>inputoutput: Input
--image_data_list
--full_prob
--dis_top_class
--dis_top_prob
post_end=>end: post_end
valid_num_cond=>condition: number of image_data_list > 0?
error=>inputoutput: error, skin area of this image too small, denote it by
--post_msg
healthy_cond=>condition: healthy prob in full_prob > 0.95?
healthy_op1=>operation: extract geometry feature vectors of image_data_list, and get
--feature_mat
healthy_op2=>operation: calculate common features dictionary, and denote it by
--post_out_feature_dict
healthy_op3=>operation: form healthy prob in full_prob to a dictionary, and denote it by
--post_out_merged_result
--post_msg = 'INFO'
disease_op1=>operation: extract geometry feature vectors of image_data_list, and get
--feature_mat
disease_op2=>operation: calculate disease features dictionary, and denote it by
--post_out_feature_dict
disease_op3=>operation: calculate similarity between post_out_feature_dict and 
pre-written disease description dictionary, denote it by
--similarity_dict
disease_op4=>operation: convex combinate similarity_dict and dis_top_prob by paramter 0.1, denote the result as
--post_out_merged_result
--post_msg = 'INFO'
post_output=>inputoutput: Output
--post_msg
--post_out_feature_dict
--post_out_merged_result
post_start->post_input->valid_num_cond
valid_num_cond(no)->error->post_output
valid_num_cond(yes)->healthy_cond
healthy_cond(yes)->healthy_op1->healthy_op2->healthy_op3->post_output
healthy_cond(no)->disease_op1->disease_op2->disease_op3->disease_op4->post_output
post_output->post_end
```

## 结果处理流程

```flow
resh_start=>start: res_handler start
resh_input=>inputoutput: Input
--msg
optional input--feature_dict
optional input--merged_result
msg_cond=>condition: is msg an error message?
confirm_cond=>condition: is the biggest prob 
in merged_result > 0.9 ?
confirm_op=>operation: set degree_of_confirm as 'confirm'
possible_cond=>condition: is the biggest prob 
in merged_result > 0.6 ?
possible_op=>operation: set degree_of_confirm as 'possible'
unknown_op=>operation: set degree_of_confirm as 'unknown'
resh_output=>inputoutput: Output
--msg
--degree_of_confirm default is None
--feature_dict default is None
--merged_result default is None
resh_end=>end: res_handler end
resh_start->resh_input->msg_cond
msg_cond(yes, right)->resh_output
msg_cond(no)->confirm_cond
confirm_cond(yes)->confirm_op->resh_output
confirm_cond(no)->possible_cond
possible_cond(yes)->possible_op->resh_output
possible_cond(no)->unknown_op->resh_output
resh_output->resh_end
```

![img](/home/dxh/Old/TensorFlow-Tutorials-master/myPros/git_prog/tf_setup/microscopy.png)