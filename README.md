# mouseTracking
Collection of code to analyze open field and novel object-place-context recognition test.

1. Different mouse body regions (the nose, the right ear, the left ear, the bottom, the mid tail and the tip of the tail) are detected frame by frame using
a pretrained neural network (Resnet50) using Deeplabcut. 
2. Heatmaps and traking plots are obtained starting from the DLC .csv file containing the body parts coordinates. 
3. A random forest classifier is trained to distinguish between frame of objects exploration and frames of no exploration.
