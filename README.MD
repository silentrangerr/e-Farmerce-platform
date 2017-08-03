![e-farmerce Platform](http://e-farmerce.com/logo.jpg)

Deep learning & satellite imagery are changing the way we look at agriculture. Our Primary goal is Make the Crops Searchable from Satellite Imagery and identify the Geo Location of the crops with Crop Based Deep learning Models.
We explain the e-farmerce Platform Implementation of Deep Learning in Satellite Imagery for Coconut Crop Identification


## Underlying Technology

The satellite imagery is retrieved using Google Static Maps API using Ruby and GeoJSON. The details on how to achieve this are mentioned in the ```README``` in the ```download_area``` directory.

These images are labelled using the labelling tool provided ```(gui.py)```. The tool allows a user to classify images as ```coconut farm, not coconut farm, partly coconut farm```, 

The labels and the images are then converted into a Keras friendly format, i.e., into an HDF5 file.

This HDF5 file is fed to a Convolutional Neural Network, which is a Sequential Keras model. The trained model can then be used to make predictions.

These predictions are saved to a ```.csv``` file so that they can be easily exported for visualization in other software, such as [Google Custom Maps.](
https://www.google.com/mymaps/)

## Getting Started

	git clone <this repo> 

	cd download_area/

	ruby download_area.rb 
	#to get the satellite images


If you are downloading the images for a new area, you will need to label those images. 
Labelling images will be done by using 

	python gui.py <images directory>/

The output of ```gui.py``` is a pickled python dictionary, named ```Classifications.p```.

	python create_hdf5.py <images directory>/
	#will create the HDF5 file, dataset.h5, using the images and ground truths.

	python model_train.py 
	#will train the model.

The train and validation indices of ```model_train.py``` are pickled into a dictionary in the ```output/``` folder. This folder will also contain the weights of the trained model after training is complete.

Currently, the model is configured to reduce learning rate by 0.2 if the loss plateaus for 5 epochs.
The model was trained for 250 epochs on a 3:1 train:test split, and achieved 92% accuracy on the validation set, with 80% on test set.

The train/validation/test sets for Cumbum can be visualized [here](http://bl.ocks.org/anonymous/raw/d8757753ccf9996543f2a046a5d86e8e/), or by using the ```Cumbum.json``` file.



	python model_predict.py <test images directory>/
	#will generate predictions as a .csv file.

## Notes

The Neural Network was trained using [Floydhub's](http://floydhub.com) free GPU offering. On average, it took 1 hour for our models, which were roughly 10MB in size.