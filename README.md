# hello-world
just new repository

   Copyright 2020 Parisa Mousavi
   
   This is a test to creat a conflict

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
   
Hi Everybody
I am Parisa from Austria
I am student in master degree Energy systems Engineering.

# Python code

import numpy as np
import matplotlib.pyplot as plt
from scipy import optimize


def f(x):
    return 924*x**6 - 2772*x**5 + 3150*x**4 - 1680*x**3 + 420*x**2 - 42*x + 1


def df(x):
    return 5544*x**5 - 13860*x**4 + 12600*x**3 - 5040*x**2 + 840*x - 42


# part1
X = np.linspace(0, 1, 200, endpoint=True)

for x in X:
    print(x, f(x))

plt.plot(X, f(X), X, 0*f(X))
plt.xlabel('x')
plt.ylabel('f(x)')
plt.title('Plot of Polynomial Function')
plt.show()

# part2
# rough values taken as initial estimates of zero positions(solutions) for the Newton's Method
x0s = [0.02, 0.17, 0.38, 0.62, 0.82, 0.98]
for x in x0s:
    root = optimize.newton(f, x, fprime=df, full_output=False)
    print('root is at = ', root)







