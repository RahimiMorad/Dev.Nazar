# Dev.Nazar
Gravity and ACF 


// single text fiels gravity

// add_filter('gform_field_value_gender_data', 'gender_data_function');
// function gender_data_function($value)
// {
//     return 'hellloooo_gender';
// }

add_filter('gform_pre_render_2', 'populate_posts');
add_filter('gform_pre_validation_2', 'populate_posts');
add_filter('gform_pre_submission_filter_2', 'populate_posts');
add_filter('gform_admin_pre_render_2', 'populate_posts');
function populate_posts($form)
{
    foreach ($form['fields'] as &$field) {
        if ($field->id == 4) {
			$terms = get_terms([
				'taxonomy' => 'questions',
				'hide_empty' => false,
			]);
			$choices = array();
			foreach($terms as $term)
			{
				$choices[] = array('text' =>$term->name , 'value' =>$term->name);
			}
        $field->placeholder = 'Select a Question';
            $field->choices = $choices;
		}
		
		if ($field->id == 5) {
			$terms = get_terms([
				'taxonomy' => 'voters',
				'hide_empty' => false,
			]);
			$choices = array();
			foreach($terms as $term)
			{
				$choices[] = array('text' =>$term->name , 'value' =>$term->name);
			}
        $field->placeholder = 'Select a Question';
            $field->choices = $choices;
		}
		
		if ($field->id == 8) {
			$terms = get_terms([
				'taxonomy' => 'gender',
				'hide_empty' => false,
			]);
			$choices = array();
			foreach($terms as $term)
			{
				$choices[] = array('text' =>$term->name , 'value' =>$term->name);
			}
        $field->placeholder = 'Select a Question';
            $field->choices = $choices;
        }
    }

    return $form;
}

// add_shortcode(checkuser, checkuser_funct);
// function checkuser_funct(){
// !is_admin()
// }
add_action("gform_after_submission_2", "acf_post_submission", 10, 2);
function acf_post_submission ($entry, $form)
{
	$created_posts = gform_get_meta( $entry['id'], 'gravityformsadvancedpostcreation_post_id' );
	$post_id = $created_posts[0]['post_id'];
	//    var_dump($post_id);
	if($entry['4'] == '5')
	{
		$row = array(
			'cpt_survey_r_type_a_ta_q' => $entry['11'],
			'cpt_survey_r_type_a_ta_a'   => $entry['12']
		);
	}
	if($entry['4'] == '7')
	{
	$row = array(
		'cpt_survey_r_type_a_ta_q' => $entry['15'],
		'cpt_survey_r_type_a_ta_a'   => $entry['16']
	);
}
if($entry['4'] == '9')
	{
	$row = array(
		'cpt_survey_r_type_a_ta_q' => $entry['17'],
		'cpt_survey_r_type_a_ta_a'   => $entry['18']
	);
}

add_row('cpt_survey_type_a_question', $row, $post_id);

$row_b = array(
	'cpt_survey_r_type_b_tb_q' => $entry['25'],
	'cpt_survey_r_type_b_tb_a' => $entry['28'],
	'cpt_survey_r_type_b_tb_b' => $entry['29'],
	'cpt_survey_r_type_b_tb_c' => $entry['30'],
	'cpt_survey_r_type_b_tb_d' => $entry['31'],

	'cpt_survey_r_typeb_correct' => $entry['27'],
	'cpt_survey_r_typeb_url' => $entry['32'],
);

add_row('cpt_survey_type_b_question', $row_b, $post_id);
// var_dump($entry);
}
