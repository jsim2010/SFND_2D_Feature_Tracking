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
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Shi-Tomasi | 125 | 118 | 123 | 120 | 120 | 113 | 114 | 123 | 111 | 112 | tiny |
| HARRIS | 17 | 14 | 18 | 21 | 26 | 43 | 18 | 31 | 26 | 34 | small |
| FAST | 419 | 427 | 404 | 423 | 386 | 414 | 418 | 406 | 396 | 401 | small |
| BRISK | 264 | 282 | 282 | 277 | 297 | 279 | 289 | 272 | 266 | 254 | most large |
| ORB | 92 | 102 | 106 | 113 | 109 | 125 | 130 | 129 | 127 | 128 | large |
| AKAZE | 166 | 157 | 161 | 155 | 163 | 164 | 173 | 175 | 177 | 179 | some medium |
| SIFT | 138 | 132 | 124 | 137 | 134 | 140 | 137 | 148 | 159 | 137 | some medium |

## MP.8 Performance Evaluation 2

Below is a table of the number of matched keypoints between 2 consecutive images for each valid detector and descriptor combination.

| Detector | Descriptor | 1&2 | 2&3 | 3&4 | 4&5 | 5&6 | 6&7 | 7&8 | 8&9 | 9&10 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Shi-Tomasi | BRISK | 95 | 88 | 80 | 90 | 82 | 79 | 85 | 86 | 82 |
| | BRIEF | 115 | 111 | 104 | 101 | 102 | 102 | 100 | 109 | 100 |
| | ORB | 104 | 103 | 100 | 102 | 103 | 98 | 98 | 102 | 97 |
| | FREAK | 86 | 90 | 86 | 88 | 86 | 80 | 81 | 86 | 85 |
| | SIFT | 112 | 109 | 104 | 103 | 99 | 101 | 96 | 106 | 97 |
| HARRIS | BRISK | 12 | 10 | 14 | 15 | 16 | 16 | 15 | 23 | 21 |
| | BRIEF | 14 | 11 | 15 | 20 | 24 | 26 | 16 | 24 | 23 |
| | ORB | 12 | 13 | 16 | 18 | 24 | 18 | 15 | 24 | 20 |
| | FREAK | 13 | 13 | 15 | 15 | 17 | 20 | 12 | 21 | 18 |
| | SIFT | 14 | 11 | 16 | 19 | 22 | 22 | 13 | 24 | 22 |
| FAST | BRISK | 256 | 243 | 241 | 239 | 215 | 251 | 248 | 243 | 247 |
| | BRIEF | 320 | 332 | 299 | 331 | 276 | 327 | 324 | 315 | 307 |
| | ORB | 306 | 314 | 295 | 318 | 284 | 312 | 284 | 312 | 323 | 306 | 304 |
| | FREAK | 251 | 247 | 233 | 255 | 231 | 265 | 251 | 253 | 401 |
| | SIFT | 316 | 325 | 297 | 311 | 291 | 326 | 315 | 300 | 301 |
| BRISK | BRISK | 171 | 176 | 157 | 176 | 174 | 188 | 173 | 171 | 184 |
| | BRIEF | 178 | 205 | 185 | 179 | 183 | 195 | 207 | 189 | 183 |
| | ORB | 160 | 171 | 157 | 170 | 154 | 180 | 171 | 175 | 172 |
| | FREAK | 160 | 177 | 155 | 173 | 161 | 183 | 169 | 178 | 168 |
| | SIFT | 182 | 193 | 169 | 183 | 171 | 195 | 194 | 176 | 183 |
| ORB | BRISK | 73 | 74 | 79 | 85 | 79 | 92 | 90 | 88 | 91 |
| | BRIEF | 49 | 43 | 45 | 59 | 53 | 78 | 68 | 84 | 66 |
| | ORB | 65 | 69 | 71 | 85 | 91 | 101 | 95 | 93 | 91 |
| | FREAK | 42 | 36 | 44 | 47 | 44 | 51 | 52 | 48 | 56 |
| | SIFT | 67 | 79 | 78 | 79 | 82 | 95 | 95 | 94 | 94 |
| AKAZE | BRISK | 137 | 125 | 129 | 129 | 131 | 132 | 142 | 146 | 144 |
| | BRIEF | 141 | 134 | 131 | 130 | 134 | 146 | 150 | 148 | 152 |
| | ORB | 130 | 129 | 128 | 115 | 132 | 132 | 137 | 137 | 146 |
| | FREAK | 126 | 129 | 127 | 121 | 122 | 133 | 144 | 147 | 138 |
| | AKAZE | 138 | 138 | 133 | 127 | 129 | 146 | 147 | 151 | 150 |
| | SIFT | 134 | 134 | 130 | 136 | 137 | 147 | 147 | 154 | 151 |
| SIFT | BRISK | 64 | 66 | 62 | 66 | 59 | 64 | 64 | 67 | 80 |
| | BRIEF | 86 | 78 | 76 | 85 | 69 | 74 | 76 | 70 | 88 |
| | FREAK | 65 | 72 | 64 | 66 | 59 | 59 | 64 | 65 | 79 |
| | SIFT | 82 | 81 | 85 | 93 | 90 | 81 | 82 | 102 | 104 |

# MP.9 Performance Evaluation 3

Below are 2 tables of the time it takes (in milliseconds) for keypoint detection and descriptor extraction. The descriptor extractions are run after an AKAZE keypoint detection.

| Detector | Img 1 | Img 2 | Img 3 | Img 4 | Img 5 | Img 6 | Img 7 | Img 8 | Img 9 | Img 10 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Shi-Tomasi | 14 | 11 | 11 | 11 | 10 | 9 | 10 | 10 | 11 | 11 |
| HARRIS | 13 | 11 | 10 | 10 | 10 | 22 | 10 | 12 | 15 | 15 |
| FAST | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |
| BRISK | 35 | 30 | 30 | 30 | 29 | 29 | 29 | 29 | 29 | 29 |
| ORB | 7 | 6 | 6 | 6 | 6 | 6 | 6 | 6 | 6 | 6 |
| AKAZE | 57 | 52 | 66 | 57 | 65 | 54 | 65 | 59 | 61 | 63 |
| SIFT | 75 | 72 | 82 | 69 | 76 | 70 | 73 | 69 | 68 | 71 |

| Descriptor | Img 1 | Img 2 | Img 3 | Img 4 | Img 5 | Img 6 | Img 7 | Img 8 | Img 9 | Img 10 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| BRISK | 2 | 1 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |
| BRIEF | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| ORB | 7 | 7 | 8 | 7 | 7 | 7 | 7 | 7 | 7 | 7 |
| FREAK | 63 | 34 | 37 | 34 | 36 | 35 | 35 | 44 | 41 | 37 |
| AKAZE | 54 | 42 | 46 | 64 | 61 | 63 | 53 | 53 | 52 | 68 |
| SIFT | 16 | 13 | 61 | 17 | 33 | 14 | 17 | 17 | 19 | 18 |

My first recommendation would be the FAST detector with BRIEF descriptor. This is the fastest combination, averaging about 3 milliseconds. Additionally I would recommend the 2 combinations of the ORB detector with either the BRIEF or BRISK descriptors. These combinations are still relatively fast, averaging about 7-8 milliseconds. However, they result in a significantly lower number of keypoint matches, around 65-80, compared to FAST with BRIEF, which results in around 320.  This lower number of keypoint matches means less processing is required in future steps.
