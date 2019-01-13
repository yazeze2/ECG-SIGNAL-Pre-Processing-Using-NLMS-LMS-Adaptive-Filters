# ECG-SIGNAL-Pre-Processing-Using-NLMS-LMS-Adaptive-Filters
In this project, the LMS and NLMS adaptive filters are implemented to remove the baseline wander, EMG and motion artifacts from a given signal and compare the results to deduce important distinctions regarding the respective performances.


Introduction and Motivation
According to the World Health Organization (WHO), Cardiovascular diseases (CVDs) are the number one causes of death globally. An estimated 17.7 million people died from CVDs in 2015, representing 31% of all global deaths. More people die annually from CVDs than from any other cause. One of the most widely known heart diseases reported in the medical literature is Cardiac Arrhythmias. Cardiac arrhythmias can lead to chest pain, cardiac arrest, and eventually sudden cardiac death. Therefore, it is of the utmost importance to the medical community and the general public to attempt devising a permanent solution or, at least, a low-cost and non-invasive methodology to detect arrhythmia before it progresses to cause severe consequences. Electrocardiograms (ECG) is a noninvasive and inexpensive technique commonly employed by cardiologists in their clinical practice routine. In ECG, the placement of electrodes on the skin on opposing sides of the heart enables the electrical current generated by the heart to be recorded. Each heartbeat in the cardiac cycle of an ECG waveform describes the time evolution of the heart’s electrical activity, which is made of diverse electrical depolarization-repolarization. A single cycle of the cardiac system is represented in an ECG signal as shown in figure 1 below.

![image](https://user-images.githubusercontent.com/32316270/51080660-96fba580-16a5-11e9-8456-518b2e0bf927.png)
 
Figure 1: A single cardiac cycle
ECG is frequently used to detect cardiac rhythm abnormalities, by measuring the electrical activity of the heart over a period of time. A Cardiac Arrhythmia is formally defined as the disruption of the heart’s electrical activity. Therefore, by utilizing a healthy individual’s ECG waveform as a reference, the irregularities in another ECG waveform can be detected; hence an arrythmia can be detected through carefully analyzing a given ECG recording. 
However, such recordings can be corrupted by several biological and environmental noises, which include Power Line Interference, Baseline Wander, Muscle Artifact (EMG noise), and Motion Noise. A power line noise is generated by a proximity radio-frequency, electrosurgical noise, or instrumentation noise. Baseline wander is a low frequency artifact in the ECG that arises from breathing. On the other hand, EMG and motion artifacts are generated by the movement of the subject. These corruptions cause variations in the signal which can lead to diagnostic errors, even minor fluctuations can hinder the detection of arrhythmias. Therefore, it is critical that ECG signals are carefully pre-processed first before utilizing them for any clinical application. However, we should be cautious when filtering the remaining noises from the signal as the filtering process itself can modify the signal. As it can be deduced, it is nearly impossible to mitigate the inclusion of these noises in an ECG recording, except maybe the powerline noise from air conditioning systems, which is relatively static (usually between 50-60 Hz). Traditional notch filters are ineffective against the remaining noises due to the non-static nature of the noises. 
Adaptive filtering has become an effective and popular method for pre-processing and analysis of ECG signals. It is proven in the literature that adaptive filtering shown superior performance in denoising ECG recording without modifying the true signal. However, some adaptive filters exhibit a very large computational cost although very efficient. Computational complexity is considered one of the challenges in adaptive filtering techniques. For instance, the RLS adaptive filters has shown great performance in removing motion artifacts, which is the most challenging artifact to eliminate due to its resemblance to the true ECG signal. However, the RLS algorithms’ computational complexity is in the order of O(N2) although it converges faster than most. On the other hand, adaptive filters such as LMS and NLMS have been attractive for most applications as these filters are nearly effective as RLS filters, but their computational complexity lies in the order of O(N), approximately. 
In this project, I have implemented the LMS and NLMS adaptive filters to remove the baseline wander, EMG and motion artifacts from a given signal and compared the results to deduce important distinctions between the respective performances.
Approach
The LMS algorithm is an iterative technique for minimizing the mean square error (MSE) between the primary and the reference inputs. The primary input is the given ECG recording (the signal to be filtered) and the reference input can be either a pure ECG recording or a pure noise that is in some way correlated with the noise in the primary ECG signal. It is important to note the noise and the signal of interest in the input ECG signal are considered to be uncorrelated. The LMS filter eliminates the noise from the given ECG signal when a pure noise signal is given as the reference and it extracts the signal of interest from the noisy signal when a noiseless ECG signal is used a reference. Figure 2 above demonstrates the two adaptive structures of the LMS filter.

![image](https://user-images.githubusercontent.com/32316270/51080663-a24ed100-16a5-11e9-8d6f-5d6fe8167b22.png)
 
Figure 2: Two adaptive filter structures
In part (a) of figure 2, considering the signal s1 + n1 as the primary input and n2 as the reference noise, the MSE is calculated as follows:

![image](https://user-images.githubusercontent.com/32316270/51080669-ad096600-16a5-11e9-946d-f6cf1a57c622.png)
![image](https://user-images.githubusercontent.com/32316270/51080671-b266b080-16a5-11e9-9ab3-3bc6e54586d6.png)
 
 
Minimizing E[(n1 – y)2] results in a filter output that is the best least-squares estimate of s1 – the signal of interest. In part (b) of fig2., the primary input s1 + n1 is the signal to be filtered, from one of the ECG leads and a reference signal s2 is obtained from a second lead that is noise free. The signal s1 can be extracted by minimizing the MSE between the primary and the reference inputs. Similarly, it can be shown that the MSE for this approach is given by:

![image](https://user-images.githubusercontent.com/32316270/51080672-b5fa3780-16a5-11e9-91c4-24a4767d3ba6.png)
 
In both cases, the reference signal is adaptively modified with the filter W to match the signal of interest. The weight vector W is recursively updated to generate the least squares fit of the signal of interest. In LMS the W is updated using the equation below, where X is the reference input, d is the signal to be filtered and y is the best least square estimate of d. \mu is the step-size for the update, for convergence \mu should be wisely chosen in the given interval below.

![image](https://user-images.githubusercontent.com/32316270/51080676-beeb0900-16a5-11e9-9429-fc43f371d55a.png)

![image](https://user-images.githubusercontent.com/32316270/51080677-c9a59e00-16a5-11e9-8082-fe30dd066b09.png)

![image](https://user-images.githubusercontent.com/32316270/51080684-e510a900-16a5-11e9-98d8-20ee87a7e011.png)
 
 
y\ =\ W_k^TX_k
0<\mu<2/\lambda_{max}
In LMS algorithm, a choice of larger steps sizes makes the algorithm to fluctuate widely and experience a problem with gradient noise amplification, which can be solved by the normalization of the step size. This variant of the LMS algorithm, with normalization of the step size, is called Normalized LMS (NLMS) algorithm, whose coefficient updating equation is given by: 
W_{k+1}=\ W_k+\ \mu\ast\ \frac{X_k}{\in+\left|\left|X_k\right|\right|^2}\ast e_k

Comparing NLMS with the LMS recursion, we find that the update direction in LMS is a scaled version of the reference vector X. The “size” of the change to Wk-1 will therefore be in proportion to the norm of X. In this way, a vector Xk with a large norm will generally lead to a more substantial change to Wk-1than a vector with a small norm. Such behavior can have an adverse effect on the performance of LMS. In NLMS, a correction term that is added to Wk-1 in the NLMS recursion is normalized with respect to the squared-norm of the regressor Xk. Moreover, the positive constant ∈ in the denominator avoids division by zero or by a small number when ||X|| is zero or close to zero. In addition, NLMS employs a time-variant step-size of the form μ(k) = μ/(∈ + ||Xk||2), as opposed to the constant step-size, μ, which is used by LMS. Due to these factors, NLMS is expected to exhibit a better performance and faster convergence behavior than LMS. 
The data used in this project is taken from the MIT-BIH Noise Stress Test Database[3]. The noise recordings were made using physically active volunteers and standard ECG recorders, leads, and electrodes; the electrodes were placed on the limbs in positions in which the subjects’ ECGs were not visible. The three noise records were assembled from the recordings by selecting intervals that contained predominantly baseline wander, muscle artifact, and electrode motion artifact. Noise was added beginning after the first 5 minutes of each record, during two-minute segments alternating with two-minute clean segments. Figure 3 below exhibits the clean and noisy signals used in this project. The signal-to-noise ratios (SNRs) of all the signals used in this project is provided in this database as well.
 
Figure 4: Clean and Noisy signal from MIT-BIH Database
As mentioned above, the noise is embedded after 5 minutes and every other 2 minutes after as shown here. As such, the signal in the first 2 minutes (after the 5min.) is extracted and used here as the primary noisy input for analysis. Moreover, the pure baseline wander, EMG and motion artifact noise signals, which are used to construct the noisy signal, are also provided in the dataset.
In the NLMS filter portion of the analysis, both the noiseless signal and pure noises were used to compare the performance of the NLMS filter against itself. When using the pure noises as a reference, the noisy signal was first filtered to eliminate the baseline wander noise from the signal. Then, the sum of the muscle and EMG artifacts was used to completely filter the noisy signal. As shown in fig 4. Below, the NLMS algorithm converged smoothly, and the noisy signal is slightly improved. To quantify this improvement the SNR values of the signals were utilized. The SNR value of the noisy signal was confirmed to increase from 2.39dB to 3.1dB.

 
Figure 4: NLMS Filter – Baseline Wander Filtered
The second filtering step of NLMS filtering was aimed at eliminating the remaining artifacts in the signal. Figure 5 below depicts the result. As it can be observed, the final output showed significant improvement, and this is reflected in an improvement of the overall SNR from 3.1dB to 3.7dB. 
 
Figure 6: NLMS Filter – Completely Filtered Signal
On the other hand, when the noiseless ECG signal is used as reference to filter, the results showed significant improvement compared to the above approach. The difference is visually detectable, and the results are given in fig. 6 below. The SNR value has also improved to 4.23dB. Although it is not possible to make any generalizations, it was interesting to notice the difference in performance of the two different approaches of the NLMS adaptive filter. However, a lot can be concluded when comparing the NLMS and LMS algorithms. 
Due to the above results, the pure ECG signal was used as a reference for the LMS adaptive filter. As expected, it was found that the NLMS filter is significantly better than that of the LMS. The output of the LMS filter is exhibited in figure 7 below.
 
Figure 7: LMS Filter – Completely Filtered Signal
Although it is challenging to visually notice the variations of the LMS and NLMS filters, the difference is apparent in the SNR values. The LMS filtered signal produced SNR value of 3.9dB as opposed to the 4.23dB final SNR value of the NLMS filter. Moreover, as depicted in figure 8 below, the NLMS converged faster than the LMS filter.

 
Figure 8: Convergence NLMS vs LMS
Conclusion
In this project, the problem of noise cancellation from ECG signal using adaptive filters was explored. Specifically, the performances of the LMS and NLMS adaptive filters were compared. The reference signals were properly chosen in such a way that the filter output is the best least squared estimate of the original ECG signal. From the simulated results, it is clear that both algorithms significantly removed the artifacts present in the ECG signal. However, it is important to note the NLMS filter performed better. The NLMS filter provided higher SNR with faster convergence rate.




















References
	Thakor, N., & Zhu, Y. (1991). Applications of adaptive filtering to ECG analysis: Noise cancellation and arrhythmia detection. IEEE Transactions on Biomedical Engineering, 38(8), 785-794. doi:10.1109/10.83591
	Sayed, Ali H.. Adaptive Filters . Wiley. Kindle Edition.
	The MIT-BIH Noise Stress Test Database, www.physionet.org/physiobank/database/nstdb/

