# Hermite function calculator

## Hermite functions
This package calculates the [Hermite functions](https://en.wikipedia.org/wiki/Hermite_polynomials#Hermite_functions),

<p align="center">
       <img src="https://github.com/Rob217/Hermite-functions/blob/master/equations/Hermite_functions.png" width="350" />
</p>
<!---
\psi_n(x) = \frac{1}{\sqrt{2^n n!}} \frac{1}{\pi^{1/4}} \text{e}^{-x^2/2} H_n(x)
-->

where `n` is a non-negative integer, `x` is a position, and `H_n(x)` are the [Hermite polynomials](https://en.wikipedia.org/wiki/Hermite_polynomials).

### Quantum harmonic oscillator
The Hermite functions are related to the wavefunctions of the [quantum harmonic oscillator](https://en.wikipedia.org/wiki/Quantum_harmonic_oscillator) via the relation

<p align="center">
       <img src="https://github.com/Rob217/Hermite-functions/blob/master/equations/QHO_wavefunctions.png" width="350" />
</p>
<!---
\psi_n^{\mathrm{QHO}}(x) = \left(\frac{1}{2 x_{\mathrm{ZP}}^2}\right)^{1/4}  \psi_n\left(\frac{x}{x_{\mathrm{ZP}} \sqrt{2}}\right)
-->

where `xZP = hbar / 2 m omega` is the zero point motion length, `hbar` is the reduced Planck constant, and `omega` is the harmonic oscillator frequency.

<p align="center">
       <img src="https://github.com/Rob217/Hermite-functions/blob/master/examples/QHO_states.png" width="450" />
</p>

## Installation

To clone from GitHub:
```Shell
$ git clone https://github.com/rob217/hermite-functions.git
```
Then to install:
```Shell
$ cd Hermite-functions
$ python setup.py install
```


## Usage

```python
>>> from hermite_functions import hermite_functions
>>> hermite_functions(5, 0) # psi_n(0) for 0 <= n <= 5
array([[ 0.75112554],
       [ 0.        ],
       [-0.53112597],
       [-0.        ],
       [ 0.45996858],
       [ 0.        ]])
>>> hermite_functions(5, 0, all_n=False) # psi_n(0) for n = 5
0.0
```

The `move_axis` option causes `hermite_functions` to move about the axes of the output (as in [`np.moveaxis`](https://numpy.org/doc/stable/reference/generated/numpy.moveaxis.html)):
```python
>>> import numpy as np
>>> x = np.mgrid[-2:3, 0:4]
>>> hermite_functions(5, x).shape
(6, 2, 5, 4)
>>> old_new_axes = ([0, 1, 2, 3], [3, 2, 1, 0])
>>> hermite_functions(5, x, move_axes=old_new_axes).shape
(4, 5, 2, 6)
```

More examples can be found in [examples](https://github.com/Rob217/Hermite-functions/tree/master/examples).


## Testing

Test scripts are provided in `test/test_hermite_functions.py`. To run using `pytest`, use:
```Shell
$ pytest # run all tests
```

## Calculation method

`hermite_functions` provides three methods for calculating the Hermite functions:
- `recursive`
    - This method is the default and should be used at all times except for testing or if `n<5` (in which case `analytic` is marginally more efficient).
    - Makes use of the recurrence relation

<p align="center">
       <img src="https://github.com/Rob217/Hermite-functions/blob/master/equations/recurrence_relation.png" width="400" />
</p>
<!---
\psi_n(x) = \sqrt{\frac{2}{n}} x \psi_{n-1}(x) - \sqrt{\frac{n-1}{n}} \psi_{n-2}(x)
-->

- `analytic`
    - This method uses the analytic expressions for `psi_n` for `0<=n<=5`

<p align="center">
       <img src="https://github.com/Rob217/Hermite-functions/blob/master/equations/first_hermite_functions.png" width="250" />
</p>
<!---
\psi_0(x) = & \pi^{-1/4} \,\mathrm{e}^{-x^2/2}
\\
\psi_1(x) = & \sqrt{2} \pi^{-1/4} \,x\, \mathrm{e}^{-x^2/2}
-->

- `direct`
    - This method directly calculates `psi_n(x)` using the definition of the Hermite function. However, this becomes intractable for large `n` due to the explicit calculation of the factorials and Hermite polynomials and so should be used just for testing.

<p align="center">
       <img src="https://github.com/Rob217/Hermite-functions/blob/master/equations/Hermite_functions.png" width="350" />
</p>
<!---
\psi_n(x) = \frac{1}{\sqrt{2^n n!}} \frac{1}{\pi^{1/4}} \text{e}^{-x^2/2} H_n(x)
-->


## Contributions

Comments and contributions are very welcome!

## License

This package is covered by the [MIT License](https://github.com/Rob217/Hermite-functions/blob/master/LICENSE).
