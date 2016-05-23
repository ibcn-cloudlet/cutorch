CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
PROJECT(THC)

FIND_PACKAGE(CUDA REQUIRED)

INCLUDE_DIRECTORIES(../../../include/TH)


SET(CMAKE_C_FLAGS "-std=c99")
SET(CUDA_NVCC_FLAGS "-Xcompiler -fPIC")
SET(src
    THCGeneral.c THCStorage.c THCStorageCopy.c THCTensor.c THCTensorCopy.c)

SET(src-cuda
  THCReduceApplyUtils.cu
  THCBlas.cu
  THCStorage.cu
  THCStorageCopy.cu
  THCTensor.cu
  THCTensorCopy.cu
  THCTensorMath2.cu
  THCTensorMathBlas.cu
  THCTensorMathCompare.cu
  THCTensorMathCompareT.cu
  THCTensorMath.cu
  THCTensorMathPairwise.cu
  THCTensorMathPointwise.cu
  THCTensorMathScan.cu
  THCTensorMathTransformReduce.cu
  THCTensorMasked.cu
  THCTensorIndex.cu
  THCTensorConv.cu
  THCTensorRandom.cu
  THCApply.cu
  THCTensorSort.cu
  )

SET(CMAKE_POSITION_INDEPENDENT_CODE ON)
CUDA_ADD_LIBRARY(THC STATIC ${src} ${src-cuda})
CUDA_ADD_CUBLAS_TO_TARGET(THC)
TARGET_LINK_LIBRARIES(THC ${CMAKE_CURRENT_SOURCE_DIR}/../../../TH/libTH.a ${CUDA_curand_LIBRARY})

INSTALL(TARGETS THC
          RUNTIME DESTINATION "bin"
          LIBRARY DESTINATION "lib"
          ARCHIVE DESTINATION "lib")

INSTALL(FILES
          THC.h
          THCGeneral.h
          THCBlas.h
          THCStorage.h
	  THCStorageCopy.h
          THCTensor.h
	  THCTensorCopy.h
          THCTensorRandom.h
          THCTensorMath.h
          THCTensorConv.h
          THCTensorSort.h
          THCApply.cuh
          THCReduce.cuh
          THCReduceApplyUtils.cuh
          DESTINATION "lib/include/THC")
