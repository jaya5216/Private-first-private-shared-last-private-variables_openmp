# Private-first-private-shared-last-private-variables_openmp
#include<stdio.h>
#include<omp.h>
#include<math.h>
int main()
{
int i, iterationResult, factor, sum;
sum = 1;
factor = 5;
//Here, i is PRIVATE because i would be the loop variable and iteration Result must store a different result for each iteration
//factor is SHARED because all the threads would use the same factor value
//sum is FIRSTPRIVATE so that it's initialized value outside the parallel block can be used in all the threads as the initial value which would otherwise return undefined
//iterationResult is LASTPRIVATE as I can get the value from the thread that's running the last iteration of the for loop which would otherwise just be undefined if it were private
#pragma omp parallel for default(none) private(i) shared(factor) firstprivate(sum) lastprivate(iterationResult)
for(i=0; i<10; i++)
{
iterationResult = i*factor;
sum = sum+iterationResult;
printf("iterationResult for thread no. %d is %d\n", omp_get_thread_num(), iterationResult);
}
printf("Due to lastprivate, the value of iterationResult from final iteration is preserved and it is %d", iterationResult);
return 0;
}
