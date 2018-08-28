## Features

- Complete user friendly  UI. Every thing you can using drag & drop 
- There is lot of inbuilt elements.
- Support bootsrap & lot of icons.


# Page Builder by Maatwerk Online.md

![](https://www.maatwerkonline.nl/images/logo.png)

The Page Builder contain the below things.
## Elements
By defualt plugin have lot or inbuilt elements. But there is one option by which developer can override the existing elements & also create new elements.
Existing elements you can found here.

###### maatwerkonline-page-builder/elements
If you want override element then just put the same element the theme element folder.

###### wp-content/themes/your-theme/spacer.php

Now Page Builder will execute file from the theme element folder. Now according your requirement you modify the element.

## How to create new element.

```php
<?php
/*
$pb_elements global element
In pb_elements need to insert name of the element $pb_elements['uniuqe_name'] like below 
*/
$pb_elements['icon_pb'] = array(
	'name' => __('Icon', 'onlinemarketingnl' ),
	'type' => 'block',
	'icon' => 'pi-button',
	'category' => __('Navigation', 'onlinemarketingnl' ),
	'attributes' => array(
		'icon' => array(
			'description' => __('Icon', 'onlinemarketingnl' ),
			'type' => 'icon',
		),
		'size' => array(
			'description' => __('Icon size in pixels', 'onlinemarketingnl' ),
			'type' => 'text',
		),
		'responsive' => array(
			'description' => __( 'Responsive', 'onlinemarketingnl' ),
			'tab' => __('Visibility', 'onlinemarketingnl' ),
			'default' => '',
			'type' => 'select',
			'values' => array(
				'' =>  __( 'Always Visible', 'onlinemarketingnl' ),
				'hidden-xs-down' => __( 'Hidden Extra Small down', 'onlinemarketingnl' ),
			),
		),'conditional'=>array('description'=>'Conditional','tab'=>'Visibility','type'=>'conditional'),
		'animation' => array(
			'default' => '',
			'description' => __('Entrance Animation', 'onm_textdomain' ),
			'type' => 'select',
			'values' => array(
				'' => __('None', 'onm_textdomain' ),
				'fadeIn' => __('fadeIn', 'onm_textdomain' ),
				'flipXIn' => __('flipXIn', 'onm_textdomain' )
			),
			'tab' => __('Animation', 'onm_textdomain' ),
		),
		'timing' => array(
			'default' => 'linear',
			'description' => __('Timing Function', 'onlinemarketingnl' ),
			'type' => 'select',
			'values' => array(
				'linear' => __('Linear', 'onlinemarketingnl' ),
				'ease' => __('Ease', 'onlinemarketingnl' ),
				'ease-in' => __('Ease-in', 'onlinemarketingnl' ),
				'ease-out' => __('Ease-out', 'onlinemarketingnl' ),
				'ease-in-out' => __('Ease-in-out', 'onlinemarketingnl' ),
			),
			'tab' => __('Animation', 'onlinemarketingnl' ),
		),
		'duration' => array(
			'description' => __('Animation Duration (ms)', 'onlinemarketingnl' ),
			'default' => '1100',
			'tab' => __('Animation', 'onlinemarketingnl' ),
		),
		'delay' => array(
			'description' => __('Inner Delay (ms)', 'onlinemarketingnl' ),
			'default' => '0',
			'tab' => __('Animation', 'onlinemarketingnl' ),
		),
		'id' => array(
			'description' => __('ID', 'onlinemarketingnl' ),
			'info' => __('Additional custom ID', 'onlinemarketingnl' ),
			'tab' => __('Advanced', 'onlinemarketingnl' ),
		),
		'class' => array(
			'description' => __('Class', 'onlinemarketingnl' ),
			'info' => __('Additional custom classes for custom styling', 'onlinemarketingnl' ),
			'tab' => __('Advanced', 'onlinemarketingnl' ),
		),
	)
);

/*
Function needs to create for output the elements
It should as pb_{unique_name}_shortcode
*/
function pb_icon_pb_shortcode( $atts, $content = null ) {
	extract( shortcode_atts( pb_extract_attributes('icon_pb'), $atts ) );
	$conditional_logic_out="";
	if(isset($atts['conditional'])){ $conditional_logic_out = evaluate_conditional_logic($atts['conditional']); }
	$style = array();
	$style[] = $conditional_logic_out;
	$applied_style = "";
	if(count($style)>0){ $applied_style = "style='".implode(" ", $style)."'"; }
	//echo $applied_style;	
	$id_out = ($id!='') ? 'id='.$id.'' : '';
	$classes[] = $responsive;
	$classes[] = $class;
	if($animation!=''){
		$classes[] = 'animo';
		$duration = ($duration!='') ? $duration : '1000';
		$animation_out = 'data-animation="'.esc_attr($animation).'" data-duration="'.esc_attr($duration).'" data-delay="'.esc_attr($delay).'" data-delay_px="'.esc_attr($delay_px).'" ';
	}

	$icon_out = ($icon!='') ? '<div '.$applied_style.'><i style="font-size:'.$size.'px" class="'.esc_attr($icon).'"></i></div>' : '';

	$return = ' '.$icon_out.' ';

	if ( ($conditional==1 && $if==$is && $then=='hide') || ($conditional==1 && $if!=$is && $then=='show')) {
		return false;
	} else {
		return $return;
	}
}
?>
```

## Active callback.

These the settings which is executed when a event triggerd on the elements. I have shown you a example in which I have applied the active_callback on the border. So, If the value of border "rounded" then it will hide the class & id else it will show the fields.

```php
<?php 
		'border' => array(
			'description' => __('Border', 'onlinemarketingnl' ),
			'default' => 'None',
			'type' => 'select',
			'values' => array(
				'sharp' =>  __('Sharp', 'onlinemarketingnl' ),
				'rounded' =>  __('Rounded', 'onlinemarketingnl' ),
				'rounded-top' =>  __('Rounded top', 'onlinemarketingnl' ),
				'rounded-right' =>  __('Rounded right', 'onlinemarketingnl' ),
				'rounded-bottom' =>  __('Rounded bottom', 'onlinemarketingnl' ),
				'rounded-left' =>  __('Rounded left', 'onlinemarketingnl' ),
				'rounded-circle' =>  __('Rounded circle', 'onlinemarketingnl' ),
			),
			'active_callback'    => array(
				array(
					'setting'  => array('id','class'),
					'operator' => '==',
					'value'    => 'rounded',
				)
			)
		),
?>
```


