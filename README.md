# **Finding Lane Lines on the Road**
# Project One - basic lane finding in images/video
In the first project of the udacity self-driving car course we had to find lane markings in pictures and then videos of streets.

# all code in P1.ipynb

For this assignment detection of the lane marking a five step process:

1. Convert the image to grayscale
2. Use a gaussian filter to blur/de-noise the picture
3. Run canny edge detection
 * this is an algorithm to use gradient changes to find edges
4. Run a hough transform on the images to find lines
 * this is neat trick that transforms your image from image space to parameters space and then votes to determine where lines are
5. Draw the lines and then merge them into the original image

## The basics

The first 4 steps were fairly straightforward. Some observations:
1. The parabola mask is hard coded and it would be interesting to have to calibrate live cameras in real cars
 * this is probably only done at the factory or dealer but would still require a lot better method than my eyeballing
 * I'll be interested to see later how we adjust for curved roads and intersections as well as roads with only one line, for example down the center
2. the various algorithms are easy to spend a lot of time trying to dial in - change one thing at a time!

## Drawing the lines

Interestingly I was able to get very reasonable lines drawn with just the basics but they had gaps in the pictures were a bit jumpy in the video.

I smoothed them out with some simple things:
1) Sort into left and right lines (rising vs falling)
* remember you are 'upside down' in images, I tripped on this and confused myself for a while
2) get slope and the b value (y=mx+b) - although you can just calculate b at the end
* get rid of any were the slope is higher than .5 - they are perpendicular so not lane lines
3) average out the values of x,y,b, slope for each side
4) plot with as little math to get the new x value against the bottom edge

[example output]: ./test_images_out/solidWhiteCurve.jpg "White Curve"

## Shortcomings
1) this only works on more or less straight sections of road
2) this requires painted lines, were I live this would be limited
3) so far we don't deal with intersections
4) going over hills and horizons would be tricky especially at high speed

## Potential Improvements
1) Add smoothing to get the jitter out of the video - maybe discard outlying slopes, etc
2) The lines are too long in the video, better masking would be a huge help
3) Adjust the horizon mask based on speed
4) better python skills
5) Some filtering for high/low contrast changes when, for example, passing under a bridge

## Videos
In the bonus video I was able to try in some of my initial atttempts but when I added in the smoothing I broke something and am out of time. I think the reflections and glare on the hood and side of the road were causing issues. I was going to have a look at the greayscale conversion to see if we could tone down the brightspots.

### video example
[example output]: https://github.com/tpak/CarND-LaneLines-P1/blob/master/test_videos_output/solidWhiteRight.mp4 "Solid White Line Video"

[example output]: ./test_videos_output/solidYellowLeft.mp4 "Solid Yellow Line Video"
