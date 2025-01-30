% Collect some bottles at your home, partially and completely filled with some liquid.
% Take pictures of these bottles.
% Write a program which can label only incompletely filled bottles and test on these pictures.
%--------------------------------------------------------------------------------------------%
% CODE
clc;
clear;
close all;

% Read the image
[file, path] = uigetfile({'*.*'}, 'Select an Image File');
if isequal(file, 0)
    disp('User canceled file selection.');
    return;
end
img = imread(fullfile(path, file));

% Convert to grayscale
gray_img = rgb2gray(img);

% Apply edge detection to detect bottle edges
edges = edge(gray_img, 'Canny');

% Detect circular objects (bottle tops) using Hough Transform
[centers, radii] = imfindcircles(gray_img, [20 100], 'ObjectPolarity', 'dark', 'Sensitivity', 0.9);

% Convert image to binary for segmentation
bw = imbinarize(gray_img, 'adaptive');

% Perform morphological operations to refine bottle detection
bw = imclose(bw, strel('disk', 5));
bw = imfill(bw, 'holes');

% Label connected components
props = regionprops(bw, 'BoundingBox', 'Area');

% Create a figure to show results
figure, imshow(img);
hold on;

% Process each detected bottle
for i = 1:length(props)
    bbox = props(i).BoundingBox;
    subImage = imcrop(gray_img, bbox); % Crop bottle region

    % Determine the liquid level using horizontal intensity variations
    profile = mean(subImage, 2);
    [~, liquidLevel] = min(profile); % Finding the lowest intensity (liquid)

    % Define threshold for incomplete filling (if liquid level > 70% of bottle height)
    if liquidLevel < 0.7 * size(subImage, 1)
        rectangle('Position', bbox, 'EdgeColor', 'r', 'LineWidth', 2);
        text(bbox(1), bbox(2) - 10, 'Partially Filled', 'Color', 'red', 'FontSize', 12);
    end
end

hold off;
title('Detected Partially Filled Bottles');
