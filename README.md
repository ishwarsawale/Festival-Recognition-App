# Festival-Recognition-App
	This app can Identify following festivals
	   1. Holi
	   2. Eid
	   3. Diwali
	   4. Birthday
	   5. Marriage

## Retrain Inception v5 for our classifier

### To retrain Inception Model for new categories
1.  make dir tf_files in your $HOME/

2.  download required data and arrange as following


    $HOME/tf_files

    			/mydata/Diwali

                 /mydata/Holi

                 /mydata/Birthday

                 /mydata/Marriage

                 /mydata/Eid

3.  Install docker just google for it

4.  cd $HOME/tf_files, download retrain script


		curl -O https://raw.githubusercontent.com/tensorflow/tensorflow/r1.1/tensorflow/examples/image_retraining/retrain.py

5.  now run docker, make sure you have good internet connection
		
			docker run -it \
			  --publish 6006:6006 \
			  --volume ${HOME}/tf_files:/tf_files \
			  --workdir /tf_files \
			  tensorflow/tensorflow:nightly-devel

6.  once you are in docker run following command to retrain network
	
		  python /tensorflow/tensorflow/examples/image_retraining/retrain.py \
		  --bottleneck_dir=bottlenecks \
		  --model_dir=inception \
		  --summaries_dir=training_summaries/long \
		  --output_graph=retrained_graph.pb \
		  --output_labels=retrained_labels.txt \
		  --image_dir=mydata

### To optimized our retrained model for Android Platform

7.  once you finished training git clone into $HOME

    	git clone https://github.com/googlecodelabs/tensorflow-for-poets-2

8.  now cd into $HOME/tensorflow-for-poets-2/ and run following command

   		 cp -r ~/tf_files .

9.  now run new docker 


		   docker run -it \
		  --publish 6006:6006 \
		  --volume ${HOME}/tensorflow-for-poets-2:/tensorflow-for-poets-2 \
		  --workdir /tensorflow-for-poets-2 \
		  tensorflow/tensorflow:nightly-devel

10. optimized graph as follows


		  python -m tensorflow.python.tools.optimize_for_inference \
		  --input=tf_files/retrained_graph.pb \
		  --output=tf_files/optimized_graph.pb \
		  --input_names="Cast" \
		  --output_names="final_result"

11. now we create rounded graph
      
		  python -m scripts.quantize_graph \
		  --input=tf_files/optimized_graph.pb \
		  --output=tf_files/rounded_graph.pb \
		  --output_node_names=final_result \
		  --mode=weights_rounded



# Changes in android demo code

12. now copy rounded graph and into your asset folder

    	cp tf_files/rounded_graph.pb tf_files/retrained_labels.txt android/assets/ 

13. now in android/assets folder rename rounded_graph to retrained_graph

    		cd $HOME/tensorflow-for-poets-2/android/assets

    mv rounded_graph.pb retrained_graph.pb

14. Open in Android Studion and rebuild 

15. deploy on phone

## My App Demo 

<p align="center">
<img src="https://user-images.githubusercontent.com/15515106/28714448-22fc3a7a-73b1-11e7-9bcd-ce165a1a9b5d.png" height="600" width="400">
<img src="https://user-images.githubusercontent.com/15515106/28714449-232670b0-73b1-11e7-9c3a-5d861a7cbd20.png" height="600" width="400">
<img src="https://user-images.githubusercontent.com/15515106/28714450-2333e826-73b1-11e7-8ec1-ec547c853933.png" height="600" width="400">
<img src="https://user-images.githubusercontent.com/15515106/28714451-23525e28-73b1-11e7-81ac-1a92830e0cd5.png" height="600" width="400">
<img src="https://user-images.githubusercontent.com/15515106/28714454-23673d5c-73b1-11e7-8204-7b0b0dd79183.png" height="600" width="400">
<img src="https://user-images.githubusercontent.com/15515106/28714453-23662ca0-73b1-11e7-97bc-943146d9c416.png" height="600" width="400">
<img src="https://user-images.githubusercontent.com/15515106/28714452-23533bea-73b1-11e7-9cca-3fce6a7f332f.png" height="600" width="400">
<img src="https://user-images.githubusercontent.com/15515106/28714455-236b8ea2-73b1-11e7-818b-9c189909a33f.png" height="600" width="400">
<img src="https://user-images.githubusercontent.com/15515106/28714456-23847642-73b1-11e7-90be-2e26afb47a06.png" height="600" width="400">
<img src="https://user-images.githubusercontent.com/15515106/28714457-238970b6-73b1-11e7-9707-b57470e36f00.png" height="600" width="400">
</p>

