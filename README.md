# Get-Biggest-Three-Rhombus-Sums-in-a-Grid

You are given an m x n integer matrix grid​​​.

A rhombus sum is the sum of the elements that form the border of a regular rhombus shape in grid​​​. The rhombus must have the shape of a square rotated 45 degrees with each of the corners centered in a grid cell. Below is an image of four valid rhombus shapes with the corresponding colored cells that should be included in each rhombus sum:


Note that the rhombus can have an area of 0, which is depicted by the purple rhombus in the bottom right corner.

Return the biggest three distinct rhombus sums in the grid in descending order. If there are less than three distinct values, return all of them.

 

Example 1:


Input: grid = [[3,4,5,1,3],[3,3,4,2,3],[20,30,200,40,10],[1,5,5,4,1],[4,3,2,2,5]]
Output: [228,216,211]
Explanation: The rhombus shapes for the three biggest distinct rhombus sums are depicted above.
- Blue: 20 + 3 + 200 + 5 = 228
- Red: 200 + 2 + 10 + 4 = 216
- Green: 5 + 200 + 4 + 2 = 211
Example 2:


Input: grid = [[1,2,3],[4,5,6],[7,8,9]]
Output: [20,9,8]
Explanation: The rhombus shapes for the three biggest distinct rhombus sums are depicted above.
- Blue: 4 + 2 + 6 + 8 = 20
- Red: 9 (area 0 rhombus in the bottom right corner)
- Green: 8 (area 0 rhombus in the bottom middle)
Example 3:

Input: grid = [[7,7,7]]
Output: [7]
Explanation: All three possible rhombus sums are the same, so return [7].
 

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 50
1 <= grid[i][j] <= 10^5

diag=[[0]*51 for _ in range(100)]
antid=[[0]*51 for _ in range(100)]
OFFSET=50
class Solution:
    def getBiggestThree(self, grid: List[List[int]]) -> List[int]:
        def rhombusSum(i, j, d):
            if d==0: return grid[i][j]
            l, r, u, b=j-d, j+d, i-d, i+d
            i0, i1=u-j+OFFSET, i-l+OFFSET
            Sum=diag[i0][r+1]-diag[i0][j]
            Sum+=diag[i1][j+1]-diag[i1][l]

            j0, j1=u+j, b+j
            Sum+=antid[j0][i]-antid[j0][u+1]
            Sum+=antid[j1][b]-antid[j1][i+1]
            return Sum

        m, n=len(grid), len(grid[0])
        for i, row in enumerate(grid):
            for j, x in enumerate(row):
                i0, j0=i-j+OFFSET, i+j
                diag[i0][j+1]=diag[i0][j]+x
                antid[j0][i+1]=antid[j0][i]+x

        dM=min(m, n)>>1
        x=[-1]*3
        for d in range(dM+1):
            for i in range(d, m-d):
                for j in range(d, n-d):
                    y=rhombusSum(i, j, d)
                    if y==x[0] or y==x[1] or y==x[2]: continue
                    if y>x[0]:
                        x[2]=x[1]
                        x[1]=x[0]
                        x[0]=y
                    elif y>x[0]:
                        x[2]=x[1]
                        x[1]=x[0]
                        x[0]=y
                    elif y>x[1]:
                        x[2]=x[1]
                        x[1]=y
                    elif y>x[2]:
                        x[2]=y

        for i in range(2, -1, -1):
            if x[i]==-1: x.pop()

        return x
        
