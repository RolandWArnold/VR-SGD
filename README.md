# VR-SGD

A demo for VR-SGD(Comparing to some major algorithms).

Method "VR-SGD" is described in the paper: "VR-SGD: A Simple Stochastic Variance Reduction Baseline for Machine Learning", Fanhua Shang, Member, IEEE, Kaiwen Zhou, James Cheng, Ivor W. Tsang,
Lijun Zhang, Member, IEEE, and Dacheng Tao, Fellow, IEEE

## Usage

All algorithms are implemented in C++ including SAGA, SVRG, Prox-SVRG, Katyusha, VR-SGD, and all parameters can be passed through MATLAB.

To run the demo in MATLAB, first run `mex_all` in the MATLAB terminal to generate the mex file.(Note that the compiler should support `c++11`)

Determine all parameters in a MATLAB file and run the algorithms implemented in C++ by passing parameters through `Interface`, here is a piece of sample code:

```matlab
% load dataset variable X,y

algorithm = 'VR_SGD'; % SAGA / SVRG / Prox_SVRG / Katyusha / VR_SGD
loop = int64(passes / 3); % loop count for Prox_SVRG

passes = 240; % total passes of train set, must be a multiple of 3

model = 'logistic'; % least_square / svm / logistic

regularizer = 'L2'; % L1 / L2 / elastic_net
lambda1 = 10^(-6); % L2 / elastic_net parameter
lambda2 = 10^(-5); % L1 / elastic_net parameter

L = (0.25 * max(sum(X.^2, 1)) + lambda1); % logistic regression
sigma = lambda1; % For Katyusha / SAGA in SC case, Strong Convex Parameter
step_size = 1 / (5 * L);

init_weight = zeros(Dim, 1); % Initial weight

is_sparse = issparse(X);
result = Interface(X, y, algorithm, model, regularizer, init_weight, lambda1, L, step_size, loop, is_sparse, Mode, sigma, lambda2);

```

## Demos
One can run `demo_dense` in the MATLAB terminal, a small demo using dense dataset Adults(a9a)  from [LIBSVM Data](https://www.csie.ntu.edu.tw/~cjlin/libsvmtools/datasets/), to generate a plot shown as below.

Test environment is MATLAB R2017a with GCC-4.9, Ubuntu 16.04 LTS.

```bash
>> demo_dense
Model: L2-logistic
Algorithm: SAGA
Time: 3.919867 seconds
Algorithm: SVRG
Time: 3.026332 seconds
Algorithm: Prox_SVRG
Time: 3.111989 seconds
Algorithm: Katyusha
Time: 5.324524 seconds
Algorithm: VR_SGD
Time: 3.126436 seconds
```

![](https://raw.githubusercontent.com/jnhujnhu/VR-SGD/master/Adult_L2.png)

There is also a demo for sparse dataset rcv1(binary) from [LIBSVM Data](https://www.csie.ntu.edu.tw/~cjlin/libsvmtools/datasets/), generate a plot as below by running `demo_sparse` in the MATLAB terminal.

```bash
>> demo_sparse
Model: L2-logistic
Algorithm: SAGA
Time: 7.143901 seconds
Algorithm: SVRG
Time: 5.322789 seconds
Algorithm: Prox_SVRG
Time: 5.626229 seconds
Algorithm: Katyusha
Time: 9.928631 seconds
Algorithm: VR_SGD
Time: 5.828326 seconds
```

![](https://raw.githubusercontent.com/jnhujnhu/VR-SGD/master/RCV1_L2.png)
