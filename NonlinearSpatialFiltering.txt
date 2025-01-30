% Nonlinear Spatial Filtering - Example
%--------------------------------------%
f = imread("cktboard.png");  % Read the original image
fn = imnoise(f, 'salt & pepper', 0.2);  % Add salt-and-pepper noise

% Convert to grayscale if the image is RGB
if size(fn, 3) == 3
    fn_gray = rgb2gray(fn);
else
    fn_gray = fn;
end

gm = medfilt2(fn_gray, [3 3]);  % Apply 3x3 median filtering
gms = medfilt2(fn_gray, [3 3], 'symmetric');  % Symmetric padding

% Display results
figure, imshow(f), title('Original Image');
figure, imshow(fn), title('Noisy Image');
figure, imshow(gm), title('Median Filtered Image');
figure, imshow(gms), title('Symmetric Padded Image');
