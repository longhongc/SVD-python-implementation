# SVD-python-implementation
An implementation of Single Value Decomposition(SVD) in python

## What is SVD
SVD is a kind of decomposition technique that can be used on matrix in any size even non-square matrix.  

![image](https://user-images.githubusercontent.com/28807825/154822066-514bb426-d92c-490e-a518-fd4130168a86.png)  
If A matrix is a mxn matrix, then U matrix is a mxm orthogonal matrix and V is also a nxn orthogonal matrix.   
Sigma is a mxn matrix, and the non-zero value inside sigma is called the singular values of this matrix.   
U and V are also known as the left and right singular vectors of A. 

## How to compute SVD
### Step 1. Calculate U or V
U and V can be computed by the following equation  
![image](https://user-images.githubusercontent.com/28807825/154822194-0d1c33a4-400f-414d-9228-fe2da26ceb9c.png)

The equations are the eigen-decomposition of AA_T and A_TA.  
Because AA_T and A_TA are symmetric matrix, the eigenvalues are guaranteed to be real value.   
The square root of the eigenvalues here are the singular value mentioned in above.    
And U and V are the eigenvectors which will be orthogonal to each other for each distinct eigenvalue.

### Step 2. Calculate the other one
However, the computed U and V cannot form the A we want, because the sign of U and V can be arbitrary in eigen-decomposition of A.   
The final U and V are coupled in a special way, so the above equation cannot pick out both U and V at the same time.  
Therefore, the formal way is to calculate either U or V first then decide the other one with the following equations  
![image](https://user-images.githubusercontent.com/28807825/154822393-060d5216-9d83-4ecf-89ad-0ab188937941.png)

### Step 3. Finalize the matrix and make them orthognoal matrix
Sometimes the U or V calculated through the above steps will contain non-orthogonal vectors or zero column vectors.  
This is due to the fact that mxn matrix can only provide a maximum amount of orthognal vectors of min(m, n). 

For example, if A is a 2x4 matrix, and U is calculated through step1. When V is calculated through step 2, two columns of V will become zero.
Even if the V is calculated first, the first step cannot guarantee the orthognoality of V because of two zero eignevalues. 

Therefore the last step is to make U or V orthogonal. 
This can be done through calculating a system of linear equations. 
If U is 2x2 and V is 4x4, and we assume that V has two non-orthogonal vectors witch is the last two vectors. 
These two vectors can be calculated by finding the solution of equations that makes the dot product with the first two vector zero.

Assuem V looks like this  
![image](https://user-images.githubusercontent.com/28807825/154822971-f7575735-f688-4af7-a20b-f8832eea3e72.png)  
Two equations can be found by calculating the dot product with the first two vectors  
![image](https://user-images.githubusercontent.com/28807825/154822988-035baa5a-b29e-483e-a216-c602e6e7c0f5.png)    
From two equations four variables, we can assign arbitrary value to the free variables. 
Here we assign the free variables to be 1.  
![image](https://user-images.githubusercontent.com/28807825/154823032-ed887645-39c1-42a8-a3ad-394e5346cb6f.png)   
Normalize this vector, and place it back to V  
![image](https://user-images.githubusercontent.com/28807825/154823108-b53f43a5-4976-49d6-9f62-0bfba75d78e2.png)  
Continue with the same process again, then the last vector can also be found. 

## Simpler flow of calculating SVD
1. Calculate U or V with the Step 1 equation
2. Calculate the other matrix with the Step 2 equation  
3. Make all matrices orthognal by solving the system of equations as Step 3 
