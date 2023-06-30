add_action( 'category_add_form_fields', 'category_add_form_fields_callback' );
function category_add_form_fields_callback() {
    $image_id = null;
    ?>

    <div id="category_custom_image"></div>
    <input 
          type="hidden" 
          id="category_custom_image_url"     
          name="category_custom_image_url">
    <div style="margin-bottom: 20px;">
        <span>Category Image </span>
        <a href="#" 
            class="button custom-button-upload" 
            id="custom-button-upload">Upload image</a>
        <a href="#" 
            class="button custom-button-remove" 
            id="custom-button-remove" 
            style="display: none">Remove image</a>
    </div>

<?php 

}


add_action( 'create_term', 'custom_create_term_callback' );

function custom_create_term_callback($term_id) {
    // add term meta data
    add_term_meta( 
        $term_id, 
        'term_image',   
        esc_url($_REQUEST['category_custom_image_url'])
    );

}

add_action ( 'category_edit_form_fields', 'category_edit_form_fields_callback', 10, 2 );

function category_edit_form_fields_callback($ttObj, $taxonomy) {

    $term_id = $ttObj->term_id;
    $image = '';
    $image = get_term_meta( $term_id, 'term_image', true );

    ?>
    <tr class="form-field term-image-wrap">
      <th scope="row"><label for="image">Image</label></th>
	<td>
        <?php if ( $image ): ?>
        <span id="category_custom_image">
           <img src="<?php echo $image; ?>" style="width: 100%"/>
        </span>
        <input 
           type="hidden" 
           id="category_custom_image_url" 
           name="category_custom_image_url">
                
        <span>
           <a href="#" 
             class="button custom-button-upload" 
             id="custom-button-upload" 
             style="display: none">Upload image</a>
           <a href="#" class="button custom-button-remove">Remove image</a>                    
        </span>
        <?php else: ?>
        <span id="category_custom_image"></span>
        <input 
            type="hidden" 
            id="category_custom_image_url" 
            name="category_custom_image_url">
        <span>
           <a href="#" 
              class="button custom-button-upload" 
              id="custom-button-upload">Upload image</a>
           <a href="#" 
              class="button custom-button-remove" 
              style="display: none">Remove image</a>
        </span>
        <?php endif; ?>
        </td>
    </tr>
        
    <?php
}

add_action( 'edit_term', 'edit_term_callback' );

function edit_term_callback($term_id) {
    $image = '';
    $image = get_term_meta( $term_id, 'term_image' );

    if ( $image )
    update_term_meta( 
        $term_id, 
        'term_image', 
        esc_url( $_POST['category_custom_image_url']) );

    else
    add_term_meta( 
        $term_id, 
        'term_image', 
        esc_url( $_POST['category_custom_image_url']) );

}

function custom_category_rewrite_rule() {
    add_rewrite_rule('^([^/]+)/?', 'index.php?category_name=$matches[1]', 'top');
}
add_action('init', 'custom_category_rewrite_rule');