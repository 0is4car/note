# 堆

arena and heap are more physical concept. however bins are more logical.

no two free chunks can be adjunct or they would be combined, this means physical adjunct. Two physically adjunct chunks probably are not in a bin.

| bin type | number | linking method | chunk size | Coalescing | first call |
| :--- | :--- | :--- | :--- | :--- | :--- |
| fast | 10 | single link\(LIFO\) | 8-16-24-.... | no combine | turn to small |
| unsorted | 1 | double | all sizes in one | no say\(guess not\) |  |
| small | 62 | double\(FIFO\) | 8-16-24-... | combine | turn to unsort |
| big | 62 | double | 512-512+64...2560,2560+512 and desc in one bin | combine | turn to bigger-&gt;turn to top |

