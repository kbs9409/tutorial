# 20131079 Compiler Project 2

## Main Idea of Your Solution
  1. **IMPORTANT** : Basic rules are Same as Project 1, therefore I will focus on the differences from Project1 (For more details, refer to Project 1)
  2. In **declaration statement**, store(save) **the values of array variable id**, **the size(number) of each dimension of array**
  3. After storing, assign real size of each dimension of id which is array variable to *size\[\]\[\]*
    * For example, a\[10\]\[20\]; -> id\[n\] = "a", size\[n\]\[0\] = 80, size\[n\]\[1\]
  4. In **L** production, store the first array id's index and the number of current temp variable to use in declaration statement later

## Code Description
* **Assignment.l**:
  1. Unlike project 1, '\n' is ignored and ';' represents the end of the statement, instead.
  2. deliever **int**, **char**, **float**, **double** as **TYPE** token to **yacc**.

* **Assignment.y**:
  - **Declarations**
	+ **ic** : id\[\] counter
	+ **sc** : (t)size\[\] counter
	+ **cur** : index of current array id
	+ **curd** : dimension of current array id
	+ **d\_c** : temp variable which contains total offset of array id in declaration statement
	+ **d\_id** : index of array id in declaration statement
	+ **id\[\]** : array id storage
	+ **tsize\[\]** : temp size storage of each dimension of each array id / 1st dimension size stored in tsize\[0\]
	+ **size\[\]\[\]** : aggregate array size storage of each dimension of each array id / highest dimension size stored in **size\[n\]\[0\]**

	* New token : **TYPE** / New non-terminal : **L** which is casted to string type

  - **Grammar rules**
	+ **S** : Statement consists of declaration and assignment statement
	+ **decl** : Call **assign(TYPE)**
	+ **lcon** : Store array id and temp size
	+ **asgns** : Assignment statement consists of assignment expression
	+ **asgn** : Print assignment expression using **d\_id** and **d\_c**
	+ **E**
	  1. **asgn** : Do nothing
	  2. **L** : Similar to above productions such as **E + E**
	+ **L**
	  1. **L \[ E \]** : Call **printL(E)** and store current temp variable
	  2. **ID \[ E \]** : Call **printID(ID, E)** and store current temp variable

  - **Auxiliary functions**
    + **assign(TYPE)**
	  1. Set **len** according to **TYPE**
	  2. Store **len** in **size\[ic\]\[0\]**
	  3. For each dimension, current dimension size = previous dimension size * current dimension length
	  4. increment **ic** and reset **sc**
	+ **printL(E)**
	  1. Print temp variables about current array index
	  2. If printing is finished and current step is declaration statement, set **d\_c** and **d\_id** to use at declaration statement
	  3. Set **buf** as temp variable
	+ **printID(ID, E)**
	  1. Similar to **printL()**
	  2. Before print, find current array id and its highest dimension

## Detailed Algorithm
* The algorithm is already described in above.

* To sum up briefly,
  1. Basic rules are same as Project 1
  2. Ignore '\n' and use ';' as a delimiter, instead
  3. Use some counters and storages(arrays) to store array id, size, and dimension
  4. Calculate each dimension size of each array id and store them
  5. Using this size storage, generate temp variables to make 3-address code

## anything else...

* If you have any question, contact me (kbs9409@unist.ac.kr)
