#include <stdio.h>

/*
 * Parâmetros de entrada:
 * K -> número de recuros (CPU, discos, etc)
 * N -> número de terminais
 * Z -> tempo de pensar
 * S -> vetor contendo tempo médio de serviço de cada recurso
 * V -> vetor contendo número de visitas em cada recurso
 * Quantidade de itens em S e V será igual a  K
 * */
void exactMVA(int K, int N, double Z, const double * S, const double * V) {
    FILE * arq = fopen("exactMVAResultsQ4.txt", "wt");

    double queueLen[K], responseTime[K], resourceThroughput[K], totalResponseTime, systemThroughput;
    char * resourceName[] = {"CPU", "Disco A", "Disco B"};
    for (int i = 0; i < K; i++) {
        queueLen[i] = 0;
    }

    for (int n = 1; n <= N; n++) {
        printf("N = %d\n", n);
        fprintf(arq,">>>>>>>>>>>>>>>>>>>>>>>>> N = %d <<<<<<<<<<<<<<<<<<<<<<<<<\n", n);

        for (int i = 0; i < K; ++i) {
            responseTime[i] = S[i] * (1 + queueLen[i]);
            printf("Response time %s = %.3f \n", resourceName[i], responseTime[i]);
            fprintf(arq,"Response time %s = %.3f \n", resourceName[i], responseTime[i]);
        }

        totalResponseTime = 0;
        for (int i = 0; i < K; i++) {
            totalResponseTime = totalResponseTime + (responseTime[i] * V[i]);
        }

        systemThroughput = (double) n / (totalResponseTime + Z);

        printf("System response time: %.3f \n", totalResponseTime);
        printf("System throughput: %.3f \n", systemThroughput);

        fprintf(arq,"System response time: %.3f \n", totalResponseTime);
        fprintf(arq,"System throughput: %.3f \n", systemThroughput);


        for (int i = 0; i < K; i++) {
            queueLen[i] = systemThroughput * V[i] * responseTime[i];
            resourceThroughput[i] = systemThroughput * V[i];
            printf("Queue length %s = %.3f \n", resourceName[i], queueLen[i]);
            printf("Throughput %s = %.3f \n", resourceName[i], resourceThroughput[i]);
            fprintf(arq,"Queue length %s = %.3f \n", resourceName[i], queueLen[i]);
            fprintf(arq,"Throughput %s = %.3f \n", resourceName[i], resourceThroughput[i]);
        }
    }
    fclose(arq);
}

/*
 * Função responsável por retornar o valor absoluto de um double
 * */
double absValue(double x) {
    return (x > 0) ? x : x * (-1.0);
}

/*
 * Função responsável por retornar o valor máximo da diferença entre as filas
 * */
double findMax(int K, const double * queueLen, const double * auxQueueLen) {
    double sub = auxQueueLen[0] - queueLen[0];
    double absMax = absValue(sub);

    for (int i = 1; i < K; i++) {
        sub = auxQueueLen[i] - queueLen[i];
        sub = absValue(sub);
        if (sub > absMax) {
            absMax = sub;
        }
    }

    return absMax;
}

/*
 * Parâmetros de entrada:
 * K -> número de recuros (CPU, discos, etc)
 * N -> número de terminais
 * Z -> tempo de pensar
 * S -> vetor contendo tempo médio de serviço de cada recurso
 * V -> vetor contendo número de visitas em cada recurso
 * Quantidade de itens em S e V será igual a  K
 * */
void estimatedMVA(int K, int N, double Z, const double * S, const double * V, const double epsilon) {
    FILE * arq = fopen("estimatedMVAResultsQ3.txt", "wt");

    double queueLen[K], responseTime[K], resourceThroughput[K], totalResponseTime, systemThroughput, auxQueueLen[K];
    char * resourceName[] = {"CPU", "Disco A", "Disco B"};
    int countIteration = 0;
    for (int i = 0; i < K; i++) {
        queueLen[i] = (double) N / (double) K;
    }

    do {
        countIteration++;
        printf("Iteration %d\n", countIteration);
        fprintf(arq,">>>>>>>>>>>>>>>>>>>>>>>>> Iteration %d <<<<<<<<<<<<<<<<<<<<<<<<<\n", countIteration);

        for (int i = 0; i < K; i++) {
            auxQueueLen[i] = queueLen[i];
        }

        for (int i = 0; i < K; ++i) {
            responseTime[i] = S[i] * (double) (1.0 + ((N - 1.0) / (double) N) * queueLen[i]);
            printf("Response time %s = %.3f \n", resourceName[i], responseTime[i]);
            fprintf(arq,"Response time %s = %.3f \n", resourceName[i], responseTime[i]);
        }

        totalResponseTime = 0;
        for (int i = 0; i < K; i++) {
            totalResponseTime = totalResponseTime + (responseTime[i] * V[i]);
        }

        systemThroughput = (double) N / (totalResponseTime + Z);

        printf("System response time: %.3f \n", totalResponseTime);
        printf("System throughput: %.3f \n", systemThroughput);

        fprintf(arq,"System response time: %.3f \n", totalResponseTime);
        fprintf(arq,"System throughput: %.3f \n", systemThroughput);

        for (int i = 0; i < K; i++) {
            queueLen[i] = systemThroughput * V[i] * responseTime[i];
            resourceThroughput[i] = systemThroughput * V[i];
            printf("Queue length %s = %.3f \n", resourceName[i], queueLen[i]);
            printf("Throughput %s = %.3f \n", resourceName[i], resourceThroughput[i]);
            fprintf(arq,"Queue length %s = %.3f \n", resourceName[i], queueLen[i]);
            fprintf(arq,"Throughput %s = %.3f \n", resourceName[i], resourceThroughput[i]);
        }
    } while (findMax(K, queueLen, auxQueueLen) > epsilon);

    fclose(arq);
}

// EXEMPLO SLIDE
//int main() {
//    int N = 20, K = 3;
//    double Z = 4.0;
//    double s_cpu = 0.125, s_a = 0.3, s_b = 0.2;
//    double v_cpu = 16.0, v_a = 10.0, v_b = 5.0;
//    double S[] = {s_cpu, s_a, s_b}, V[] = {v_cpu, v_a, v_b};
//    estimatedMVA(K, N, Z, S, V, 0.01);
//    return 0;
//}


/*
 * Lista de Exercios 2
 * Execicio 3, item 4
 * s_cpu = D_cpu / V_cpu = 0.08/14 = 0.0057
 */
//int main() {
//    int N = 4, K = 3;
//    double Z = 0.0;
//    double s_cpu = 0.0057, s_a = 0.03, s_b = 0.025;
//    double v_cpu = 14.0, v_a = 5.0, v_b = 8.0;
//    double S[] = {s_cpu, s_a, s_b}, V[] = {v_cpu, v_a, v_b};
//    //exactMVA(K, N, Z, S, V);
//    estimatedMVA(K, N, Z, S, V, 0.001);
//    return 0;
//}

/*
 * Lista de Exercios 2
 * Execicio 4, letras (j) e (k)
 */
//int main() {
//    int N = 30, K = 3;
//    double Z = 5.0;
//    double s_cpu = 0.04, s_a = 0.03, s_b = 0.025;
//    double v_cpu = 25.0, v_a = 20.0, v_b = 4.0;
//    double S[] = {s_cpu, s_a, s_b}, V[] = {v_cpu, v_a, v_b};
//    // Letra J
//    exactMVA(K, N, Z, S, V);
//    // Letra K
//    //estimatedMVA(K, N, Z, S, V, 0.001);
//    return 0;
//}

