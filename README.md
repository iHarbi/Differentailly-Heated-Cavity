# mainCode NV_Solver
The main code begins with defining various parameters such as domain dimensions, grid resolution, fluid properties, and simulation settings. It then initializes the mesh and staggered grids for velocity components, as well as variables for pressure, temperature, and intermediate calculations.
In the main temporal loop of the program, the solver iterates in time until either reaching a steady-state solution or a predefined simulation time. Within each time step, the solver performs the following operations:
	Velocity Prediction: Predictor steps are applied to predict the velocity field based on the previous state and the calculated convection-diffusion operator from the previous iterations. The predictor step uses either the Adam-Bashforth or Forward Euler method to advance the velocity field in time while considering the effects of buoyancy forces induced by temperature gradients.
	Temperature Update: The energy equation is solved to update the temperature field. Like the velocity prediction, the temperature field is advanced in time using either the Adam-Bashforth or Forward Euler method, considering convective and diffusive heat transfer processes.
	Pressure Calculation: Pressure is solved using a linear system derived from the discretized momentum equations, ensuring divergence-free velocity fields.
	Velocity Correction: Corrector steps are applied to update the velocity field based on the pressure field obtained in the previous step, ensuring mass conservation.
	Convergence Check: At the end of each iteration, the solver checks for convergence by comparing the current velocity and temperature fields with the previous ones. If the solution has not converged yet, the loop continues; otherwise, it exits.
Throughout the simulation, various utility functions are used to initialize variables, perform operations on staggered grids, calculate convection-diffusion operators, and write simulation results to output files in CSV format. Additionally, the code implements methods to handle boundary conditions, CFL condition calculation, and mass balance evaluation.

Finally, the simulation results, including velocity, pressure, and temperature fields, are exported to CSV files for further analysis or visualization. 


# Class PHI:
Within the "PHI" class, the header defines private attributes representing velocity components (u and v): representing the horizontal and vertical components of the velocity being convected, temperature fields: previous and current fields, and parameters related to mesh size and domain dimensions. 
Public member functions are provided for initializing, setting boundary conditions, and applying various advection schemes such as Central Difference Scheme (CDS), Upwind Scheme, Quick Scheme, and Second-Order Upwind (SOU) Scheme. Getter functions are also included to access the velocity and temperature fields. Additionally, the class defines constructors and a destructor for proper initialization and memory management.
# Class mainMesh:
The "MAINMESH " Class file provides essential structures and functions for managing mesh data in simulations. It begins with declaring structs for defining positions, areas, and cells within the mesh. The Position struct stores X and Y positions for faces and cell centroids, while the Area struct allocates positions of faces (East, West, North, and South). The Cell struct contains volume data and parameters essential for the numerical solver. Additionally, the header defines structs for staggered velocities in both X and Y directions. Within the "mainMesh" class, constructors are provided for initializing mesh dimensions, and member functions are implemented for accessing mesh properties such as cell volumes, positions, and areas. Methods for checking mesh consistency and affirming the right calculations are also included.
#Class xStaggered:
The "xStaggered.h" object encapsulates functionality for managing staggered mesh data. It begins by including necessary libraries and headers, including "mainMesh.h", to access mesh-related structures and functions; on which the staggering operations are based. The object defines structs for staggered mesh properties such as cell volumes, areas, positions, and mass flow rates across the faces in the X direction. Within the "xStaggered" class, member functions are provided for staggering operations, initializing and setting mass flow rates, as well as handling velocity boundary conditions. Constructors and destructors are implemented for proper initialization and cleanup of resources.
# Class yStaggered:
Almost identical functionalities as the above-mentioned xStaggered Class,  allowing access to cell volumes, areas, positions, and mass flow rates. 
# Classes CSVMATLAB & CSVWriter
The "CSVMATLAB.h" C++ object encapsulates functionalities for writing matrices to CSV files. The class provides a constructor that accepts a filename as input, allowing users to specify the name of the CSV file to be written. The main function, "writeMatrix," takes a two-dimensional vector representing a matrix as input and writes its contents to the specified file in CSV format. Each element of the matrix is separated by a comma, and each row is terminated by a newline character. Error handling is implemented to check if the file can be opened for writing. 
The "CSVWriter.h" C++ object provides functionalities for writing velocity data to a CSV. It relies on the "mainMesh.h" header for accessing position-related structures: nodes coordinates. The class constructor takes a filename as input. The main function, "writeMatrix," accepts a two-dimensional vector representing velocity data and a Position struct containing mesh position information. It then writes the velocity data along with the corresponding mesh positions to the specified file in CSV format. Each row of the CSV file corresponds to a single data point, with velocity components (U, V, and W) alongside the X and Y coordinates. This process of file output allows visualization using Opensource Packages (e.g., ParaView).
