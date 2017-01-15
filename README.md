#Introduction
This is a MATLAB simulation program for compressed-sensing sigma-delta analog to digital converter (ADC).

The idea of compressed-sensing comes from [basis pursuit denoising (BPDN)](https://en.wikipedia.org/wiki/Basis_pursuit_denoising) problem in machine learning. Basically, this problem has three equivalent forms:

1. Basis pursuit denoising:<br />Minimize <img src="http://www.sciweavers.org/tex2img.php?eq=%5Cleft%20%5C%7C%20x%20%5Cright%20%5C%7C_1&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt="\left \| x \right \|_1" width="39" height="19" /> subject to <img src="http://www.sciweavers.org/tex2img.php?eq=%5Cleft%20%5C%7C%20A%20%5Coverrightarrow%7Bx%7D%20-%20%5Coverrightarrow%7By%7D%20%20%5Cright%20%5C%7C_2%20%5Cleq%20%20%5Csigma%20&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt="\left \| A \overrightarrow{x} - \overrightarrow{y}  \right \|_2 \leq  \sigma " width="135" height="35" />
2. Basis pursuit:<br />Minimize <img src="http://www.sciweavers.org/tex2img.php?eq=%5Cleft%20%5C%7C%20x%20%5Cright%20%5C%7C_1&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt="\left \| x \right \|_1" width="39" height="19" /> subject to <img src="http://www.sciweavers.org/tex2img.php?eq=A%20%5Coverrightarrow%7Bx%7D%20%3D%20%5Coverrightarrow%7By%7D%20&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt="A \overrightarrow{x} = \overrightarrow{y} " width="75" height="26" />
3. Lasso:<br />Minimize <img src="http://www.sciweavers.org/tex2img.php?eq=%5Cleft%20%5C%7C%20A%20%5Coverrightarrow%7Bx%7D%20-%20%5Coverrightarrow%7By%7D%20%20%5Cright%20%5C%7C_2&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt="\left \| A \overrightarrow{x} - \overrightarrow{y}  \right \|_2" width="100" height="35" /> subject to <img src="http://www.sciweavers.org/tex2img.php?eq=%5Cleft%20%5C%7C%20x%20%20%5Cright%20%5C%7C_1%20%5Cleq%20%20%5Ctau%20&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt="\left \| x  \right \|_1 \leq  \tau " width="74" height="19" />

Machine learning researchers have developed various kinds of algorithms to solve BPDN problem. Some basic textbook materials can be found in Kevin P. Murphy's [Machine Learning: A Probabilistic Perspective](https://www.amazon.com/Machine-Learning-Probabilistic-Perspective-Computation/dp/0262018020/ref=sr_1_1?ie=UTF8&qid=1484504311&sr=8-1&keywords=machine+learning+a+probabilistic+perspective) CHapter 13.2.3 sparse linear models.

In this project, we are going to use three algorithms implmente in MATLAB:

1. SPGL1: [Spectrial Projected Gradient for L1 minimization](https://github.com/mpf/spgl1)<br /><br />This algorithm is developed by Professor Michael P. Friedlander fron University of British Columbia. The details can be found in his paper [Probing the Pareto frontier for basis pursuit solutions](https://www.cs.ubc.ca/~mpf/pubs/probing-the-pareto-frontier-for-basis-pursuit-solutions/). As a general comment on this implementation, it is an iterative algorithm that can solve large scale BPDN reconstruction problem. However, even for small scale problem it still runs for many iterations and takes a relative long time to find the root.
2. OMP: Orthogonal Matching Pursuit<br /><br /> This algorithm is proposed by [Stephane G. Mallat] (https://www.di.ens.fr/~mallat/papiers/MallatPursuit93.pdf) in 1993. It is a greedy search algorithm on BPDN least square equation. It is suitable for small scale problem when dimension of X is small and computing requirement is affordable. In this scenario, it is much faster than SPGL1 but we do observe that it has some accuracy problem. Due to its greedy nature, OMP sometimes could not find all the features of X while SPGL1 could. In this project, we use [Stephen Becker](https://www.mathworks.com/matlabcentral/fileexchange/32402-cosamp-and-omp-for-sparse-recovery)'s implementation in MATLAB.
3. CoSaMP:Compressive Sampling Matched Pursuit<br /><br />This algorithm is from [D. Needell, J. A. Tropp](https://arxiv.org/pdf/0803.2392v2.pdf). At hearts, this algorithm is greedy pursuits but it also incorporates ideas from the combinatorial algorithms to guarantee speed and to provide rigorous error bounds. The details can be found in the paper. In this project, we use [Stephen Becker](https://www.mathworks.com/matlabcentral/fileexchange/32402-cosamp-and-omp-for-sparse-recovery)'s implementation in MATLAB.
