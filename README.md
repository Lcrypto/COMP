#Compressed-Sensing Sigma Delta ADC with SPG, OMP and CoSaMP
This is a MATLAB simulation program for compressed-sensing sigma-delta analog to digital converter (ADC).

##Introduction to Basis Pursuit Denoising

The idea of compressed-sensing comes from [basis pursuit denoising (BPDN)](https://en.wikipedia.org/wiki/Basis_pursuit_denoising) problem in machine learning. Basically, this problem has three equivalent forms:

1. Basis pursuit denoising:
<p align="center">
Minimize <img src="http://www.sciweavers.org/upload/Tex2Img_1484544777/render.png" align="center" border="0" alt="\left \| x \right \|_1" width="39" height="19" /> subject to <img src="http://www.sciweavers.org/upload/Tex2Img_1484545044/render.png" align="center" border="0" alt="\left \| A \overrightarrow{x} - \overrightarrow{y}  \right \|_2 \leq  \sigma " width="135" height="35" />
</p>
2. Basis pursuit:
<p align="center">
Minimize <img src="http://www.sciweavers.org/upload/Tex2Img_1484545104/render.png" align="center" border="0" alt="\left \| x \right \|_1" width="39" height="19" /> subject to <img src="http://www.sciweavers.org/upload/Tex2Img_1484545131/render.png" align="center" border="0" alt="A \overrightarrow{x} = \overrightarrow{y} " width="75" height="26" />
</p>
3. Lasso:
<p align="center">
Minimize <img src="http://www.sciweavers.org/upload/Tex2Img_1484545155/render.png" align="center" border="0" alt="\left \| A \overrightarrow{x} - \overrightarrow{y}  \right \|_2" width="100" height="35" /> subject to <img src="http://www.sciweavers.org/upload/Tex2Img_1484545178/render.png" align="center" border="0" alt="\left \| x  \right \|_1 \leq  \tau " width="74" height="19" />
</p>

Machine learning researchers have developed various kinds of algorithms to solve BPDN problem. Some basic textbook materials can be found in Kevin P. Murphy's [Machine Learning: A Probabilistic Perspective](https://www.amazon.com/Machine-Learning-Probabilistic-Perspective-Computation/dp/0262018020/ref=sr_1_1?ie=UTF8&qid=1484504311&sr=8-1&keywords=machine+learning+a+probabilistic+perspective) Chapter 13.2.3 sparse linear models.

In this project, we are going to use three algorithms implmente in MATLAB:

1. SPGL1: [Spectrial Projected Gradient for L1 minimization](https://github.com/mpf/spgl1)<br /><br />This algorithm is developed by Professor Michael P. Friedlander fron University of British Columbia. The details can be found in his paper [Probing the Pareto frontier for basis pursuit solutions](https://www.cs.ubc.ca/~mpf/pubs/probing-the-pareto-frontier-for-basis-pursuit-solutions/). As a general comment on this implementation, it is an iterative algorithm that can solve large scale BPDN reconstruction problem. However, even for small scale problem it still runs for many iterations and takes a relative long time to find the root.
2. OMP: Orthogonal Matching Pursuit<br /><br /> This algorithm is proposed by [Stephane G. Mallat] (https://www.di.ens.fr/~mallat/papiers/MallatPursuit93.pdf) in 1993. It is a greedy search algorithm on BPDN least square equation. It is suitable for small scale problem when dimension of X is small and computing requirement is affordable. In this scenario, it is much faster than SPGL1 but we do observe that it has some accuracy problem. Due to its greedy nature, OMP sometimes could not find all the features of X while SPGL1 could. In this project, we use [Stephen Becker](https://www.mathworks.com/matlabcentral/fileexchange/32402-cosamp-and-omp-for-sparse-recovery)'s implementation in MATLAB.
3. CoSaMP:Compressive Sampling Matched Pursuit<br /><br />This algorithm is from [D. Needell, J. A. Tropp](https://arxiv.org/pdf/0803.2392v2.pdf). At hearts, this algorithm is greedy pursuits but it also incorporates ideas from the combinatorial algorithms to guarantee speed and to provide rigorous error bounds. The details can be found in the paper. In this project, we use [Stephen Becker](https://www.mathworks.com/matlabcentral/fileexchange/32402-cosamp-and-omp-for-sparse-recovery)'s implementation in MATLAB.

##Introduction to Compressed Sampling
Here, I will present a highly simplified introduction to the compressed sampling. For more details and rigorous mathematical proof, please refer to the paper [An Introduction To Compressive Sampling](http://dsp.rice.edu/sites/dsp.rice.edu/files/cs/CSintro.pdf).

1. What is signal sensing or sampling:<br /><br />In this artical, we will discuss sensing mechanisms in which information about a time domain signal f(t) is obtained by a series of recording values <p align="center"> <img src="http://www.sciweavers.org/upload/Tex2Img_1484582835/render.png" align="center" border="0" width="219" height="19" /> </p> In another word, we correlate signal f(t) with a series of pre-defined standard signals <img src="http://www.sciweavers.org/upload/Tex2Img_1484582992/render.png" align="center" border="0" alt="\varphi_{k}(t)" width="44" height="19" />, for which we have various kinds of choices. For example, if <img src="http://www.sciweavers.org/upload/Tex2Img_1484582992/render.png" align="center" border="0" alt="\varphi_{k}(t)" width="44" height="19" /> are Dirac delta functions (spikes), then we are simpling recording f(t) with its discrete sampled values y. If <img src="http://www.sciweavers.org/upload/Tex2Img_1484582992/render.png" align="center" border="0" alt="\varphi_{k}(t)" width="44" height="19" /> are sine wave functions <img src="http://www.sciweavers.org/upload/Tex2Img_1484583389/render.png" align="center" border="0" alt="sin( \omega _k t)" width="72" height="18" /> in which <img src="http://www.sciweavers.org/upload/Tex2Img_1484583501/render.png" align="center" border="0" alt="\omega_k = k \times  \omega " width="93" height="18" />, we are actually transforming f(t) into frequency domain and this process is called discrete Fourier transformation. We give these pre-defined standard signals <img src="http://www.sciweavers.org/upload/Tex2Img_1484582992/render.png" align="center" border="0" alt="\varphi_{k}(t)" width="44" height="19" /> a new name: basis function. If they are orthonormal basis, we call them orthobasis. The sine wave functions described above is an orthobasis example.
2. Sparse signal and sparsity:<br /><br />Mathematically speaking, in previous section, we have a signal f in t-domain (note this domain can be extended to other domain, for example, two-dimension space domain for images) and we expand f in an orthonormal basis <img src="http://www.sciweavers.org/upload/Tex2Img_1484585153/render.png" align="center" border="0" alt=" \Phi  = [ \varphi _1,  \varphi _2, ... ,   \varphi _3 ]" width="156" height="19" /> as follows:<p align="center"><img src="http://www.sciweavers.org/upload/Tex2Img_1484592153/render.png" align="center" border="0" alt="f(t) =  \sum_{i=1} ^ n x_i   \varphi  _i(t)" width="133" height="50" /></p>By this expansion, we transform f from t-domain to its reciprocal domain and represent it with a series of coefficient <img src="http://www.sciweavers.org/upload/Tex2Img_1484585499/render.png"align="center" border="0" width="106" height="19" />. f(t) may have vary complex form in t-domain, however, it could be sparse in its reciprocal domain. That is, many entry of <img src="http://www.sciweavers.org/upload/Tex2Img_1484585806/render.png" align="center" border="0" alt="x_i" width="18" height="15" /> is actually 0 or very close to 0 and only S entries are nonzero. We denote these nonzero enties in a set X<sub>s</sub> and call S as the sparsity of the original signal f(t). To make it more clear, we consider <img src="http://www.sciweavers.org/upload/Tex2Img_1484591388/render.png" align="center" border="0" alt="f_s(t)" width="39" height="19" /> obtained by keeping only S nonzero terms. Here, we define<p align="center"><img src="http://www.sciweavers.org/upload/Tex2Img_1484591858/render.png" align="center" border="0" alt="f_s: = \Psi X_S" width="79" height="19" /></p>where from here, X<sub>s</sub> is the vector of  coefficients 
