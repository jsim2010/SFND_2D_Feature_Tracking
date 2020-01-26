# Writeup

## MP.0 Mid-Term Report

You're reading it.

## MP.1 Data Buffer Optimization

To implement a ring buffer, I first checked the size of the data buffer. If the size was equal to the limit, then I removed the first, and oldest element, prior to adding to new one at the end.

## MP.2 Keypoint Detection

I implemented the HARRIS detector in `detKeypointHarris()`. This worked by first calculating the Harris corner response for each pixel. Then I found the pixels that were the local maxima of their area and marked those as keypoints.

Each of the other detectors were implemented by using the appropriate OpenCV `FeatureDetector` in `detKeypointsModern()`.

## MP.3 Keypoint Removal

I created a `Rect` with the appropriate bounds, then only kept all keypoints that were contained by the `Rect`.

## MP.4 Keypoint Descriptors

I implemented each of the descriptors by using the appropriate OpenCV `DescriptorExtractor` in `descKeypoints()`.

## MP.5 Descriptor Matching

In `matchDescriptors()`, I implemented both the FLANN matching and k-nearest-neighbor selection. The FLANN matching was implemented by creating an OpenCV `DescriptorMatcher` that was based on FLANN matching. For the k-nearest-negighbor selection, I used OpenCV's `knnMatch()` function.

## MP.6 Descriptor Distance Ratio

In `matchDescriptors()`, I implemented a filter to only keep the matches that were within the range specified by the provided `minDescDistRatio`.

## MP.7 Performance Evaluation 1

Below is a table the number of keypoints that each detector was able to find.

| Detector | Img 1 | Img 2 | Img 3 | Img 4 | Img 5 | Img 6 | Img 7 | Img 8 | Img 9 | Img 10 | Neighboorhood size distribution |
| === | === | === | === | === | === | === | === | === | === | === | === |
| Shi-Tomasi | 125 | 118 | 123 | 120 | 120 | 113 | 114 | 123 | 111 | 112 | tiny |
| HARRIS | 17 | 14 | 18 | 21 | 26 | 43 | 18 | 31 | 26 | 34 | small |
| FAST | 419 | 427 | 404 | 423 | 386 | 414 | 418 | 406 | 396 | 401 | small |
| BRISK | 264 | 282 | 277 | 297 | 279 | 289 | 272 | 266 | 254 | most large |
| ORB | 92 | 102 | 106 | 113 | 109 | 125 | 130 | 129 | 127 | 128 | large |
| AKAZE | 166 | 157 | 161 | 155 | 163 | 164 | 173 | 175 | 177 | 179 | some medium |
| SIFT | 138 | 132 | 124 | 137 | 134 | 140 | 137 | 148 | 159 | 137 | some medium |

## MP.8 Performance Evaluation 2

Below is a table of the number of matched keypoints between 2 consecutive images for each detector and descriptor combination.

| Detector | Descriptor | 1&2 | 2&3 | 3&4 | 4&5 | 5&6 | 6&7 | 7&8 | 8&9 | 9&10 |
| === | === | === | === | === | === | === | === | === | === | === |
| Shi-Tomasi | BRISK | 95 | 88 | 80 | 90 | 82 | 79 | 85 | 86 | 82 |
| | BRIEF | 115 | 111 | 104 | 101 | 102 | 102 | 100 | 109 | 100 |
