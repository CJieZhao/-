# OpenBLAS 函数说明
---
1. cblas_dgemm 函数计算的公式为 `C := alpha * A * B + beta * C`

```
void cblas_dgemm(
OPENBLAS_CONST enum CBLAS_ORDER Order,
OPENBLAS_CONST enum CBLAS_TRANSPOSE TransA, OPENBLAS_CONST enum CBLAS_TRANSPOSE TransB, 
OPENBLAS_CONST blasint M, OPENBLAS_CONST blasint N, OPENBLAS_CONST blasint K,
OPENBLAS_CONST double alpha, 
OPENBLAS_CONST double *A, OPENBLAS_CONST blasint lda, 
OPENBLAS_CONST double *B, OPENBLAS_CONST blasint ldb,
OPENBLAS_CONST double beta,
double *C, OPENBLAS_CONST blasint ldc);
```

__Order__:CblasRowMajor(行序), CblasColMajor(列序)

__TransA__:A矩阵是否转置(如果有转置，MKN值是转置之后的维度, 并且根据转置之前的维度组织内存), CblasNoTrans(无转置),CblasTrans(转置)

__TransB__:B矩阵是否转置(如果有转置，MKN值是转置之后的维度, 并且根据转置之前的维度组织内存), CblasNoTrans(无转置),CblasTrans(转置)

__M__:A矩阵的行数

__N__:B矩阵的列数

__K__:A矩阵的列数同时是B矩阵的行数

__alpha__: 公式系数

__A__:A矩阵铺成一维的数组

__lda__:(TransA == CblasNoTrans) ? K : M

__ldb__:(TransB == CblasNoTrans) ? N : K

__bete__:公式系数

__C__:C矩阵，既是出参又是入参

__ldc__: 等于M