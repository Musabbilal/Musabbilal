% Parameters
SampleRate = 1e6; % Sample rate in Hz
PulseWidth = 50e-6; % Pulse width in seconds
PRF = 10e3; % Pulse repetition frequency in Hz
NumPulses = 10; % Number of pulses

% Rectangular pulse train
waveform = phased.RectangularWaveform('SampleRate', SampleRate, 'PulseWidth', PulseWidth, 'PRF', PRF, 'OutputFormat', 'Pulses', 'NumPulses', NumPulses);

% Generate the pulse train samples
pulsesamples = waveform();



% Periodic ambiguity function
fs = 200*PRF; 
F = pambgfun(pulsesamples, fs, 7, 'Cut', '2D');

% Reshaping
B = zeros(500,500);
B(1:size(F,1), 1:size(F,2)) = F;

% Plotting
figure;
plot(pulsesamples);
ylim ([-.5 1.5]);
title('Pulse Train');


% Plotting
figure; 
surf(B); 
xlabel('Time Delay');
ylabel('Doppler Shift');
zlabel('Amplitude'); 
title('PAF of Pulse Train');
