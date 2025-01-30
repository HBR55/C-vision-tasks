% Write a program which can read an image as an input and do the following automatically. Show the results of all steps.
% Find the type of image: binary, gray or RGB.
% Find the issue in image, over dark, over bright, low contrast, or normal. (Hint: can use histogram).
% Resolve the issue if any and show the final image after enhancement. 
% CODE
%------------------------------------------------------------------------------------------------------------------------%
clc;
clear;
close all;

% Read the input image
[file, path] = uigetfile({'*.*'}, 'Select an Image File');
if isequal(file, 0)
    disp('User canceled file selection.');
    return;
end
img = imread(fullfile(path, file));

% Determine image type
if islogical(img)
    img_type = 'Binary Image';
elseif ndims(img) == 2
    img_type = 'Grayscale Image';
elseif ndims(img) == 3
    img_type = 'RGB Image';
else
    error('Unknown image format.');
end

fprintf('Image Type: %s\n', img_type);

% Convert RGB to grayscale if needed
if strcmp(img_type, 'RGB Image')
    gray_img = rgb2gray(img);
else
    gray_img = img;
end

% Display original image
figure;
subplot(2,3,1);
imshow(img);
title('Original Image');

% Compute Histogram
hist_values = imhist(gray_img);
subplot(2,3,2);
imhist(gray_img);
title('Histogram');

% Analyze brightness and contrast
mean_intensity = mean(gray_img(:));
std_dev = std(double(gray_img(:)));

% Define thresholds
low_thresh = 50;
high_thresh = 200;
low_contrast_thresh = 40;

% Detect issues
if mean_intensity < low_thresh
    issue = 'Over-Dark Image';
    fprintf('Detected Issue: %s\n', issue);
    enhanced_img = imadjust(gray_img, stretchlim(gray_img), []);
elseif mean_intensity > high_thresh
    issue = 'Over-Bright Image';
    fprintf('Detected Issue: %s\n', issue);
    enhanced_img = imadjust(gray_img, stretchlim(gray_img), []);
elseif std_dev < low_contrast_thresh
    issue = 'Low Contrast Image';
    fprintf('Detected Issue: %s\n', issue);
    enhanced_img = histeq(gray_img);
else
    issue = 'Normal Image';
    fprintf('No enhancement needed.\n');
    enhanced_img = gray_img;
end

% Display enhancement results
subplot(2,3,3);
imhist(enhanced_img);
title('Enhanced Histogram');

subplot(2,3,4);
imshow(enhanced_img);
title('Enhanced Image');

