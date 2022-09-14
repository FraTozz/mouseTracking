# Mouse Tracking analysis pipeline 🐭

![image](https://user-images.githubusercontent.com/39329654/190141860-e2b390ab-eaeb-4f9a-b0bd-56c53eed8ffd.png)

This repository contains a collection of code to analyze behavior in mice.

I used this pipeline to analyze and automatically score open field and novel object-place-context recognition test in mice in order to test **episodic memory**.

It uses the following Python libraries:

- **Analysis:**
  - Pandas
  - Numpy
  - OpenCV
- **Plotting:**
  - Matplotlib
  - Seaborn

---

## Expected inputs

Behavioral videos have to be processed with [DeepLabCut](https://github.com/DeepLabCut/DeepLabCut) in order to extract coordinates for different body regions in each frame (the nose, the right ear, the left ear, the bottom, the mid tail and the tip of the tail).

![image7(1)](https://user-images.githubusercontent.com/39329654/190140101-2f009568-5188-4c5a-9b95-da4f758b0745.gif)

## General behavioral scoring

Based on the tracking results, the analysis pipeline will automatically calculate the following behavioral parameters:

- Total **distance** traveled (cm)
- Average **speed** (cm/s)
- Time spent in the **center** of the arena (s)
- Time spent in the **periphery** of the arena (s)

The pipeline will also produce **trajectories** and **heatmaps** of the mouse location during the session:

![image53](https://user-images.githubusercontent.com/39329654/190144950-05b06f9f-7256-41de-bdbf-2016f18a3cd2.png)

## Behavioral classification

To test memory retention, I exploit the spontaneous tendency of mice to explore novel objects and situations more than familiar ones. If a subject remembers to have explored the same object before it should prioritize the exploration of the other, novel one.

I quantitatively measure this tendency with a Discrimination Index defined as:

(T_novel - T_familiar) / (T_novel + T_familiar)

where:

- T_novel is the time in seconds spent exploring the novel object
- T_familiar is the time in seconds spent exploring the familiar object

Based on the distances of some body parts from the two objects, I implemented two ways to automatically classify frames as *exploration* or *non-exploration*

### Random forest classifier

I trained a **custom random forest classifier** to predict whether the mouse is exploring or not an object (bool variable) based on the distances of its body parts from the object edge.

This approach has the **advantage** that takes into consideration the actual posture of an animal and its gaze direction.

### Nose-to-Object distance

As a reference, I also implemented a classification method based only on the distance between the nose and the object's edges, as more classically done in the literature. This method requires the user to set a distance threshold to consider a frame to be an exploration one.

To determine an appropriate distance threshold, I manually scored each individual frame of several videos and used this data as ground truth. I found out that using a threshold distance of 2cm yields the most similar results to GT.
