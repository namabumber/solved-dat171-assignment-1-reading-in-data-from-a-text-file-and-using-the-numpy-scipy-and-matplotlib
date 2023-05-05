Download Link: https://assignmentchef.com/product/solved-dat171-assignment-1-reading-in-data-from-a-text-file-and-using-the-numpy-scipy-and-matplotlib
<br>
The goal of the first computer assignment is to get you comfortable in reading in data from a text file and using the NumPy, SciPy, and Matplotlib libraries. The problem to solve is to construct a graph of neighboring cities and to find the shortest path between two given cities, through the neighboring cities. <strong><sub>See the example figure at the </sub>end of this document.</strong>

Three files are supplied to test your program, with one very small file <sub>SampleCoordinates.txt </sub>suitable for testing your functions. In task 3 you need a radius that limits what is considered neighboring cities, in task 7 you need to enter start and end cities. You can find these values in the table below (the city no. corresponds to the line in the respective city file).

<table width="339">

 <tbody>

  <tr>

   <td width="162">Filename</td>

   <td width="56">Radius</td>

   <td width="71">Start city</td>

   <td width="50">End city</td>

  </tr>

  <tr>

   <td width="162">SampleCoordinates.txt</td>

   <td width="56">0.08</td>

   <td width="71">0</td>

   <td width="50">5</td>

  </tr>

  <tr>

   <td width="162">HungaryCities.txt</td>

   <td width="56">0.005</td>

   <td width="71">311</td>

   <td width="50">702</td>

  </tr>

  <tr>

   <td width="162">GermanyCities.txt</td>

   <td width="56">0.0025</td>

   <td width="71">1573</td>

   <td width="50">10584</td>

  </tr>

 </tbody>

</table>

For convenience, the task is divided into steps as listed below. Please stick to the given function prototypes, as it makes correcting reports much easier (you are of course allowed to write more sub-functions if you want). <em><sub>Functions </sub>must be documented using PyDoc.</em>

<ol>

 <li>Create the function <sub>read_coordinate_file(filename) </sub>that reads the given coordinate file and parses the results into an array of coordinates. The coordinates are expressed in the format: <sub>{a, b} </sub>where <sub>a </sub>is the latitude and <sub>b </sub>is the longitude (in degrees). Convert these using the Mercator projection to obtain the coordinates in xy-format:</li>

</ol>

<em>πb                               </em> <em>π                 πa </em>

<em><sup>x </sup></em>= <em>R</em>180<em>,           </em><em><sup>y </sup></em>= <em><sup>R</sup></em>ln tan         4 + 360

The radii in the table are given for the normalized <em>R </em>= 1. <em>(Measuring distances on a Mercator projection isn’t very accurate, but will suffice for this task.)</em>

<em>Hints: Use the function split for the strings. Convert a string to a number with the function float. Make sure to use a NumPy array to store the data for performance (i.e. the function must return a 2D NumPy array).</em>

<ol start="2">

 <li>Create the function <sub>plot_points(coord_list) </sub>that plots the data points read from the file. <em>Hints: Remember to call plt.show() after plot commands are finished.</em></li>

 <li>Create the function construct_graph_connections(coord_list, radius) that computes all the connections between all the points in <sub>coord_list </sub>that are within the radius given. Simply check each coordinate against all other coordinates to see if they are within the given radius using two nested loops. The output should contain the 2 indices in one NumPy array and the distance in another. <em>Hints: Have a look at the Python method enumerate.</em></li>

 <li>Create the function <sub>construct_graph(indices, distance, N) </sub>that constructs a sparse graph. The graph should be represented as a compressed sparse row matrix (<sub>csr_matrix </sub>in <sub>sparse</sub>). Each element at index <sub>[i, j] </sub>in the matrix should be the distance between city <sub>i </sub>and <sub>j</sub>. Construct the matrix with csr_matrix((data, ij), shape=(M, N)). <em>Hints: You need to provide a size N as input to this function. What</em></li>

</ol>

<em>is N? The SciPy manual contains examples on how to create the matrices: </em><a href="https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.csr_matrix.html">https://docs.scipy.org/doc/scipy/ref </a><a href="https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.csr_matrix.html">erence/generated/scipy.sparse.csr_matrix.html</a>

<ol start="5">

 <li>Extend <sub>plot_points(coord_list, indices) </sub>to also include the graph connections from task 3. <em>Hints: Use</em></li>

</ol>

<em>Matplotlib:s LineCollection instead of plotting the lines individually, since it is much faster.</em>

<ol start="6">

 <li>Create the function find_shortest_paths(graph, start) that finds the shortest paths from start to all other cities, using the functions in <sub>sparse.csgraph</sub>. Please make sure you properly document this function! <em>Hints: The function shortest_path finds the shortest path, it lets the user input what indices (starting nodes) the path should be computed for. This saves significant computational effort! </em><a href="https://docs.scipy.org/doc/scipy/reference/sparse.csgraph.html">https://docs.scipy.org/doc </a><a href="https://docs.scipy.org/doc/scipy/reference/sparse.csgraph.html">/scipy/reference/sparse.csgraph.html</a></li>

 <li>One of the outputs from the shortest path functions in SciPy is a “predecessor matrix”. The columns represent the predecessor when taking the shortest path to the given column index (this seems complicated, but is actually a clever way to store the shortest paths to every possible end node). Write a function compute_path(predecessor_matrix, start_node, end_node) that takes a predecessor matrix, start and end nodes, and converts it to a sequence of nodes that represent the shortest path. <em><sub>Hints: The</sub></em></li>

</ol>

<em>file SampleCoordinatex.txt should generate the sequence [0, 4, 3, 5], the total distance for this path</em>

<em>is 0.16446989717973517.</em>

<ol start="8">

 <li>Extend <sub>plot_points(coord_list, indices, path) </sub>to also include the shortest path. Make the shortest path more visible by making it stand out (thicker line width and a noticeable color for example).</li>

 <li>Record the total time for each of the functions (see the table below for reference). Which routine consumes the most time? <em>Hints: Use time.time()</em></li>

 <li>Create the function construct_fast_graph_connections(coord_list, radius) that computes all the connections between all the points in <sub>coord_list </sub>that are within the radius given. This time, use the <sub>cKDTree </sub>from SciPy to find the closest coordinates quickly. (The <sub>cKDTree </sub>is an optimized version of the <sub>KDTree</sub>.) <strong><sub>Note!</sub></strong></li>

</ol>

You must be able to swap construct_graph_connections with construct_fast_graph_connections in your code (without any other alterations)! <em>Hints: Instead of checking a coordinate against all the other coordinates,</em>

<em>we can filter the number of points by for example using the method query_ball_points(coord, radius) on the KDTree.</em>

For reference, here is the expected output for <sub>HungaryCities.txt</sub>:

Shortest path from 311 to 702 is: [311, 19, 460, 629, 269, 236, 781, 50, 193, 571, 624, 402, 370,

153, 262, 554, 126, 251, 368, 221, 827, 300, 648, 253, 836, 73, 35, 219, 503, 789, 200, 702]

Total distance: 0.1114486118821533

<strong>Additional instructions</strong>

Besides the general <em>General instructions for computer assignments </em>please consider the items below for Computer Assignment 1:

<ol>

 <li>Reading the input file:

  <ul>

   <li>Process each line as it is read.</li>

   <li>Use <sub>strip</sub>, <sub>split</sub>, and so on for processing each line, not indexing or similar.</li>

   <li>Remember to close files.</li>

  </ul></li>

 <li>As part of the report, the resulting pictures must be handed in. Make sure the pictures have a correct aspect ratio.</li>

 <li>The output of the program must include the total distance and the <em><sub>shortest path </sub></em></li>

 <li>Timing information for the major routines must be provided. On a quite old machine (Intel Core i7-2600) the following results (to one digit precision) can be used as a hint on the expected results for <sub>txt</sub>:</li>

</ol>

<table width="466">

 <tbody>

  <tr>

   <td width="420">function</td>

   <td width="46">time (s)</td>

  </tr>

  <tr>

   <td width="420">read_coordinate_file</td>

   <td width="46">0.03</td>

  </tr>

  <tr>

   <td width="420">construct_graph_connections</td>

   <td width="46">70</td>

  </tr>

  <tr>

   <td width="420">construct_graph</td>

   <td width="46">0.002</td>

  </tr>

  <tr>

   <td width="420">Task 6+7</td>

   <td width="46">0.007</td>

  </tr>

  <tr>

   <td width="420">plot_points (from task 8, excluding <sub>plt.show</sub>)</td>

   <td width="46">2</td>

  </tr>

  <tr>

   <td width="420">construct_fast_graph_connections</td>

   <td width="46">0.3</td>

  </tr>

  <tr>

   <td width="420">Running the entire program using the fast version, excluding plotting</td>

   <td width="46">1</td>

  </tr>

 </tbody>

</table>