This is the explanation of the exceptional type part of section 3.4 in the paper. All codes are in GAP-CHEVIE (https://github.com/jmichel7/gap3-jm). 

Length of good position braid representatives
------
1. We first define a function to get the eigenvalues of a Weyl group element w on its complex reflection representation. 
    
        GetEigenVal:=function(W,w)   
        return ReflectionEigenvalues(W)[Position(ChevieClassInfo(W).classtext,w)];
        end;
   
   The output of this function is a list of eigenvalues of the w-action on the complex reflection representation of W. Specifically, it will be a list of intergers in [0,1). In particular, it will be increasing while all zeroes will be at the end of the list. For example, if the output of (W,w) is [1/2,1/2,0]. then the eigenvalues of w are e^{\pi i} and e^{\2pi i}. Furthermore, the dimension of the corresponding complex eigenspace is exactly the times it occurs in the output (i.e. the dimension of the complex eigenspace corresponding to e^{\pi i} is 2). This implies that the output actually lead to an increasing sequence in (0,2\pi].

2. Next we define a function to get a basis of the complex eigenspace corresponding to eigenvalue e^{2k\pi i}.

        BaseEigen:=function(W,w,k)
        local word;
        word:=EltWord(W,w);
        return BaseMat(EigenspaceProjector(W,word,k));
        end;
   
   The output of this function a basis of the above complex eigenspace. Specifically, it will be a list of vectors which are the coordinate vectors of the basis with respect to simple roots. For example, if the output of (W,w,1/2) is [[1,0,0,0],[0,1,0,0]], then {\alpha_1, \alpha_2} is a basis of this complex eigenspace.

3. Next we define a function to check orthogonality, it has two parts.

       CheckOrthoVec:=function(W,a,b)
       local M;
       M:=TransposedMat(CartanMat(W));
       return a*M*b;
       end;
   
   The first part gives the usual inner product between a and b, they are orthogonal to each other if the output is equal to 0.

       CheckOrthoSpace:=function(W,w,k,b)
       local base,ortho,product,vec;
       base:=BaseEigen(W,w,k);
       ortho:=0;
       for vec in base do
           if CheckOrthoVec(W,vec,b)<>0 then
               ortho:=1;
           fi;
       od;
       return ortho;
       end;

   The second part determines if a vector is orthogonal to the complex eigenspace of w corresponding to eigenvalue e^{2k\pi i}. If the output is 0 then it is orthogonal to the eigenspace, otherwise it is not.

4. Next we define a function to find the minimal k for any root \alpha such that \alpha is not orthogonal to the complex eigenspace corresponding to e^{2k\pi i}. Recall our definition of "real eigenspace" in the paper, this k is also the minimal k such that \alpha is not orthogonal to the real eigenspace corresponding to e^{2k\pi i} since its complexification is the sum of the complex eigenspace cooresponding to e^{2k\pi i} and e^{2(1-k)\pi i} and k<1-k due to minimality.

       RootVal:=function(W,w,b)
       local val,eigenval;
       val:=-1;
       for eigenval in GetEigenVal(W,w) do
           if CheckOrthoSpace(W,w,eigenval,b)=1 and val=-1 then
               val:=eigenval;
           fi;
       od;
       return val;
       end;

5. Finally we define the function to compute the length of good position braid representatives of a given conjugacy class of W.

