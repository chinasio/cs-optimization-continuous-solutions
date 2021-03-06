# cs-optimization-continuous-solutions

Local searches for continuous optimization implemented in C#

# Install

```bash
Install-Package MathNet.Numerics -Version 2.4.0
Install-Package cs-optimization-continuous-solutions -Version 1.0.2
```

# Features

The package includes various local search and metaheuristics algorithms for continuous optimization:

## Numerical Methods

* Adaptive Random Search
* BFGS
* Conjugate Gradient Descent Search
* Fibonacci Search 
* Golden Section Search 
* Gradient Descent
* Newton Method
* Powell Method 
* Random Search
* SLOP

These algorithms can be found in the "ContinuousOptimization.LocalSearch" directory / namespace

## Meta-Heuristics

* Differential Evolution
* Evlutionary Programming
* Evolution Strategy
* GRASP
* Iterated Local Search 
* Nelder Mead 
* Stochastic Hill Climbing
* Tabu Search 
* Variable Neighbhorhood Search 

These algorithms can be found in the "ContinuousOptimization.MetaHeuristics" directory / namespace

# Usage

## Local Search Methods on Rosenbrock Optimization Problem

The sample codes below shows how to solve the "Rosenbrock Saddle" continuous optmization problem using ConjugateGradientSearch:

```cs
CostFunction_RosenbrockSaddle f = new CostFunction_RosenbrockSaddle();
ConjugateGradientSearch s = new ConjugateGradientSearch();
            
double[] x_0 = f.CreateRandomSolution(); // initial solution 

s.SolutionUpdated += (best_solution, step) =>
{
	Console.WriteLine("Step {0}: Fitness = {1}", step, best_solution.Cost);
};

int max_iteration = 1000;
ContinuousSoluton finalSolution = s.Minimize(x_0, f, max_iteration);
```

Where the CostFunction_RosenbrockSaddle is the cost function that is defined as below:

```cs
public class CostFunction_RosenbrockSaddle : CostFunction
{
	public CostFunction_RosenbrockSaddle()
		: base(2, -2.048, 2.048) // 2 is the dimension of the continuous solution, -2.048 and 2.048 is the lower and upper bounds for the two dimensions 
	{

	}

	protected override void _CalcGradient(double[] solution, double[] grad) // compute the search gradent given the solution 
	{
		double x0 = solution[0];
		double x1 = solution[1];
		grad[0] = 400 * (x0 * x0 - x1) * x0 - 2 * (1 - x0);
		grad[1] = -200 * (x0 * x0 - x1);
	}

	// Optional: if not overriden, the default gradient esimator will be provided for gradient computation
	protected override double _Evaluate(double[] solution) // compute the cost of problem given the solution 
	{
		double x0 = solution[0];
		double x1 = solution[1];

		double cost =100 * Math.Pow(x0 * x0 - x1, 2) + Math.Pow(1 - x0, 2);
		return cost;
	}

}
```

To replace the ConjugateGradientSearch with another search method, simply change it to another name while other part of the implementation remain the same. For example the sample code below use BFGS to solve the same problem:

```cs
CostFunction_RosenbrockSaddle f = new CostFunction_RosenbrockSaddle();
BFGS s = new BFGS();
            
double[] x_0 = f.CreateRandomSolution(); // initial solution 

s.SolutionUpdated += (best_solution, step) =>
{
	Console.WriteLine("Step {0}: Fitness = {1}", step, best_solution.Cost);
};

int max_iteration = 1000;
ContinuousSoluton finalSolution = s.Minimize(x_0, f, max_iteration);
```

## Meta-Heuristics on Rosenbrock 

The sample code below shows how to solve the Rosenbrock using differential evolution.

```cs 
CostFunction_RosenbrockSaddle f = new CostFunction_RosenbrockSaddle();

int maxIterations = 200;
DifferentialEvolution s = new DifferentialEvolution(f);

s.SolutionUpdated += (best_solution, step) =>
{
	Console.WriteLine("Step {0}: Fitness = {1}", step, best_solution.Cost);
};

ContinuousSolution finalSolution = s.Minimize(f, maxIterations);
```

The sample code below shows how to solve the Rosenbrock using evolutionary programming.

```cs
 CostFunction_RosenbrockSaddle f = new CostFunction_RosenbrockSaddle();

int maxIterations = 200;
int popSize = 100; // population Size
EvolutionaryProgramming s = new EvolutionaryProgramming(f, popSize);

s.SolutionUpdated += (best_solution, step) =>
{
	Console.WriteLine("Step {0}: Fitness = {1}", step, best_solution.Cost);
};

ContinuousSolution finalSolution = s.Minimize(f, maxIterations);
```

The sample code below show how to solve the Rosenbrock using evolution strategy.

```cs 
CostFunction_RosenbrockSaddle f = new CostFunction_RosenbrockSaddle();

int maxIterations = 200;
int mu = 30;
int lambda = 20;
EvolutionStrategy s = new EvolutionStrategy(f, mu, lambda);

s.SolutionUpdated += (best_solution, step) =>
{
	Console.WriteLine("Step {0}: Fitness = {1}", step, best_solution.Cost);
};

ContinuousSolution finalSolution = s.Minimize(f, maxIterations);
```
## More

For more examples, please refers to the cs-optimization-continuous-solutions-samples project.
