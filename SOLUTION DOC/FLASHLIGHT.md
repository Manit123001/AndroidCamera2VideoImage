```

 // TODO: Flash Button Here
    ImageView btnFlash;

    private void flashCameraCapture() {
        btnFlash = (ImageView) findViewById(R.id.btnFlash);
        turnOnTheFlash();
    }

    private void turnOnTheFlash() {
        /*
         * First check if device is supporting flashlight or not
		 */
        hasFlash = getApplicationContext().getPackageManager()
                .hasSystemFeature(PackageManager.FEATURE_CAMERA_FLASH);

        if (!hasFlash) {
            // device doesn't support flash
            // Show alert message and close the application
            AlertDialog alert = new AlertDialog.Builder(this)
                    .create();
            alert.setTitle("Error");
            alert.setMessage("Sorry, your device doesn't support flash light!");
            alert.setButton("OK", new DialogInterface.OnClickListener() {
                public void onClick(DialogInterface dialog, int which) {
                    // closing the application
                    finish();
                }
            });
            alert.show();
            return;
        }

        btnFlash.setOnClickListener(new OnClickListener() {
            @Override
            public void onClick(View view) {

                //Toast.makeText(app, "Hello Flash", Toast.LENGTH_SHORT).show();
                if (isFlashOn) {
                    // turn off flash
                    turnOffFlash();
                } else {
                    // turn on flash
                    turnOnFlash();
                }

            }
        });

    }


    // Capture Button
    public void doTakePhoto(View v) {
        mCamera.autoFocus(myAutoFocusCallback);
        Toast.makeText(app, "Capture", Toast.LENGTH_SHORT).show();
    }

    /*
         * Get the camera
         */
    private void getCamera() {
        if (camera == null) {
            try {
                camera = Camera.open();
                params = camera.getParameters();
            } catch (RuntimeException e) {
            }
        }
    }


    /*
     * Turning On flash
	 */
    private void turnOnFlash() {
        if (!isFlashOn) {
            if (camera == null || params == null) {
                return;
            }
            // play sound
            // playSound();


            params.setFlashMode(Camera.Parameters.FLASH_MODE_TORCH);
            camera.setParameters(params);
//            camera.startPreview();
            isFlashOn = true;

            // changing button/switch image
            toggleButtonImage();
        }

    }

    /*
     * Turning Off flash
     */
    private void turnOffFlash() {
        if (isFlashOn) {
            if (camera == null || params == null) {
                return;
            }
            // play sound
            //playSound();

            params.setFlashMode(Camera.Parameters.FLASH_MODE_OFF);
            camera.setParameters(params);
//            camera.stopPreview();
            isFlashOn = false;

            // changing button/switch image
            toggleButtonImage();
        }
    }


    /*
         * Toggle switch button images
         * changing image states to on / off
         * */
    private void toggleButtonImage() {
        if (isFlashOn) {
            btnFlash.setImageResource(R.drawable.tp_turn_on);
            //Toast.makeText(app, "Open", Toast.LENGTH_SHORT).show();
        } else {
            btnFlash.setImageResource(R.drawable.tp_turn_off);
            //Toast.makeText(app, "Close", Toast.LENGTH_SHORT).show();
        }
    }
    

    @Override
    protected void onPause() {
        super.onPause();
        // on pause turn off the flash
        turnOffFlash();
    }

    @Override
    protected void onResume() {
        super.onResume();

        // on resume turn on the flash
//        if(hasFlash)
//            turnOnFlash();
    }
```
