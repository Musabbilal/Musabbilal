% Parameters
SampleRate = 1e6; % Sample rate in Hz
PulseWidth = 50e-6; % Pulse width in seconds
PRF = 10e3; % Pulse repetition frequency in Hz
SweepBandwidth = 100e3;; % Sweep bandwidth in Hz
SweepDirection = 'Up'; % Sweep direction
Envelope = 'Rectangular'; % Amplitude modulation
OutputFormat = 'Pulses'; % Output format
NumPulses = 10; % Number of pulses

%LFM pulse train
waveform = phased.LinearFMWaveform('SampleRate', SampleRate, 'PulseWidth', PulseWidth, 'PRF', PRF, 'SweepBandwidth', SweepBandwidth, 'SweepDirection', SweepDirection, 'Envelope', Envelope, 'OutputFormat', OutputFormat, 'NumPulses', NumPulses);

% Generate the LFM pulse train samples
lfmsamples = waveform();

% periodic ambiguity function
fs = 200*PRF; 
F = pambgfun(lfmsamples, fs, 7, 'Cut', '2D');

% Reshaping
B = zeros(500,500);
B(1:size(F,1), 1:size(F,2)) = F;

figure;
subplot(2,1,1);
plot(real(lfmsamples));
title('LFM pulses ');

subplot(2,1,2);
plot(imag(lfmsamples));
title('Imaginary Part of LFM Samples');


%Plotting
figure; 
surf(B); 

xlabel('Time Delay');
ylabel('Doppler Shift');
zlabel('Amplitude'); 
title('PAF of LFM pulses');
