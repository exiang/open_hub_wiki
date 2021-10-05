## Introduction
To prevent bots or automated scripts from automatically submitting requests, reCaptcha can be used. This protects expensive resources like fetching SSM Reports Online. There are 3 parts to implement Google Recaptcha.

## Setup Recaptcha
### Step 1
Go to [https://www.google.com/recaptcha](https://www.google.com/recaptcha). Click "Admin Console".

![reCaptcha page](https://mymagic-misc.s3.ap-southeast-1.amazonaws.com/recaptcha_documentation/recaptcha+page.png)

### Step 2
Click Add

![reCaptcha Console](https://mymagic-misc.s3.ap-southeast-1.amazonaws.com/recaptcha_documentation/recaptcha+console.png)

### Step 3
Fill up the fields. Domains have to match the domain reCaptcha is going to be implemented into. You may use multiple domains.

![reCaptch create new](https://mymagic-misc.s3.ap-southeast-1.amazonaws.com/recaptcha_documentation/screencapture-google-recaptcha-admin-create-2021-10-04-18_16_32.png)

### Step 4
Copy site key and secret key.

![reCaptcha key](https://mymagic-misc.s3.ap-southeast-1.amazonaws.com/recaptcha_documentation/recaptcha+key.png)

### Step 5
Paste the site key and secret key into /protected/.env

![.env](https://mymagic-misc.s3.ap-southeast-1.amazonaws.com/recaptcha_documentation/recaptcha+in+dot+env.png)

## Using reCaptcha
### Step 1
Add id to your submit button. Do not use '-'. Use only alphanumeric.
example:
first-button (not valid)
first_submit (good)

```
# Inside your view file

<?php $form = $this->beginWidget(
   'ActiveForm',
   array(
      'id' => 'test-form-1',
      'enableAjaxValidation' => false,
      'htmlOptions' => array(
         'class' => 'form-horizontal crud-form',
         'role' => 'form',
         'enctype' => 'multipart/form-data'
      )
   )
); ?>

   <div class="col-sm-12 form-group text-center">
      <?php echo $form->bsBtnSubmit('Submit', array('id' => 'first_button')); ?>
   </div>

<?php $this->endWidget(); ?>		
```

### Step 2
Add reCaptcha container and pass in the id from Step 1.

```
# Inside your view file

<?php $form = $this->beginWidget(
   'ActiveForm',
   array(
      'id' => 'test-form-1',
      'enableAjaxValidation' => false,
      'htmlOptions' => array(
         'class' => 'form-horizontal crud-form',
         'role' => 'form',
         'enctype' => 'multipart/form-data'
      )
   )
); ?>

   <div class="col-sm-12 form-group">
       <?php echo Html::reCaptcha('first_button'); ?>
   </div>

   <div class="col-sm-12 form-group text-center">
      <?php echo $form->bsBtnSubmit('Submit', array('id' => 'first_button')); ?>
   </div>

<?php $this->endWidget(); ?>		
```

Optional. You may also define the id for your reCaptcha container. Same to the naming as in Step 1. Use only alphanumeric. Do not use '-'. 

```
   <?php echo Html::reCaptcha('first_button', 'first_recaptcha_container'); ?>
```
Please note that reCaptcha container is written using iframe. Therefore it is not possible to resize it.

### Step 3
Validate Request with Google reCaptcha API in controller.

The google recaptcha response will be passed to the controller and will be inside the field 'g-recaptcha-response'.
To validate this response, pass the response to HubSecure::validateRecaptchaCallback($_POST['g-recaptcha-response']).
$response->success = true means that validation is successful. if it is false, it will return a $response.

```
public function actionRecaptcha()
{
      if ($_POST['g-recaptcha-response']) {
			
         $response = HubSecure::validateRecaptchaCalback($_POST['g-recaptcha-response']);

	 $success = $response->success;

      if ($success) {
         var_dump($response);
      } else {
         echo 'failed backend validation';
      }
   }

   $model = new RecaptchaForm;

   $this->render('recaptcha', array('model' => $model));
}
```
To troubleshoot error, you may var_dump($response). The error correspond to below:

[https://developers.google.com/recaptcha/docs/verify](https://developers.google.com/recaptcha/docs/verify)



