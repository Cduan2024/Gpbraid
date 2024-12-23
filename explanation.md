This is the explanation of the exceptional type part of section 3.4 in the paper.

Length of good position braid representatives
------
1. We first define a function to get the eigenvalues of a Weyl group element w on its complex reflection representation. 
    
        GetEigenVal:=function(W,w)   
        > return ReflectionEigenvalues(W)[Position(ChevieClassInfo(W).classtext,w)];   > end;

   The output of this function is a list of eigenvalues of the w-action on the complex reflection representation of W. Specifically, it will be a list of interger ranging from [0
