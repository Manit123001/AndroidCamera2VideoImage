# AndroidCamera2VideoImage

private void initialization() {
        seekBar1=(SeekBar)findViewById(R.id.seekBar1);
        
        seekBar1.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
            @Override
            public void onProgressChanged(SeekBar seekBar, int i, boolean b) {
                zoomControls = new ZoomControls(getApplicationContext());
                zoomControls.setIsZoomInEnabled(true);
                zoomControls.setIsZoomOutEnabled(true);

                if (mCamera != null) {

                    parameter = mCamera.getParameters();

                    if (parameter.isZoomSupported()) {

                        int MAX_ZOOM = parameter.getMaxZoom();
                        int currnetZoom = parameter.getZoom();

                        parameter.setZoom(i);
                    } else
                        Toast.makeText(getApplicationContext(), "Zoom Not Avaliable", Toast.LENGTH_LONG).show();

                    mCamera.setParameters(parameter);
                }
            }

            @Override
            public void onStartTrackingTouch(SeekBar seekBar) {
               // Toast.makeText(app, "Start", Toast.LENGTH_SHORT).show();

            }

            @Override
            public void onStopTrackingTouch(SeekBar seekBar) {
                //Toast.makeText(app, "Stop", Toast.LENGTH_SHORT).show();
            }
        });

        preview.addView(zoomControls);

    }

    // TODO: EnableZoom IN / OUT
    private void enableZoom() {

        zoomControls = new ZoomControls(this);
        zoomControls.setIsZoomInEnabled(true);
        zoomControls.setIsZoomOutEnabled(true);


        zoomControls.setOnZoomInClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                zoomCamera(true);
            }
        });
        zoomControls.setOnZoomOutClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                zoomCamera(false);
            }
        });

        preview.addView(zoomControls);
    }

    /**
     * Enables zoom feature in native camera .  Called from listener of the view
     * used for zoom in  and zoom out.
     *
     * @param zoomInOrOut "false" for zoom in and "true" for zoom out
     */
    public void zoomCamera(boolean zoomInOrOut) {
        if (mCamera != null) {

            parameter = mCamera.getParameters();

            if (parameter.isZoomSupported()) {

                int MAX_ZOOM = parameter.getMaxZoom();
                int currnetZoom = parameter.getZoom();

                if (zoomInOrOut && (currnetZoom < MAX_ZOOM && currnetZoom >= 0)) {


                    if (currnetZoom < MAX_ZOOM - 50) {
                        parameter.setZoom(currnetZoom += 10);
                        //Toast.makeText(app, String.valueOf(currnetZoom), Toast.LENGTH_SHORT).show();

                    } else {
                        Toast.makeText(app, "Limit ZOOM in", Toast.LENGTH_SHORT).show();
                    }

                } else if (!zoomInOrOut && (currnetZoom <= MAX_ZOOM && currnetZoom > 0)) {
                    parameter.setZoom(currnetZoom -= 10);


                }
            } else
                Toast.makeText(getApplicationContext(), "Zoom Not Avaliable", Toast.LENGTH_LONG).show();

            mCamera.setParameters(parameter);
        }
    }
