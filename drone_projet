#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <math.h>

#define NB_Drones 10000       //Nombre total des drones
#define Seuil_distance 1.5f  //distance minimale à ne pas dépasser entre 2 drones
#define coord_max_x 2000.0f //Plage max pour X (0 à 2000 mètres)
#define coord_max_y 2000.0f //Plage max pour y (0 à 2000 mètres)
#define coord_max_z 1000.0f //Plage max pour z (0 à 1000 mètres)
#define precision 10.0f     //pour convertir rand() en float avec 1 chiffre après la virgule

typedef struct { 
    int id;   //identifiant du drone
    float x;  //coordonnée x du drone
    float y;  //coordonnée y du drone
    float z;  //coordonnée z du drone
} Drone;
int comparer_par_x(const void *a , const void *b){  // on fait un pointeur vers qlq chose qui ne sera pas modifiable en tilisant le "void" et les parametrs ds cette fct sont a et b
 
    const Drone *da = (const Drone *)a; //on convertit void* en Drone*  pour creer un pointeur vers la structure Drone 
    const Drone *db = (const Drone *)b;
    if(da->x < db ->x)  return -1;
    if (da->x> db->x) return 1;

    return 0;
}
float distance(Drone *d1, Drone *d2){  // Calculer la distance euclidienne 3D entre 2 drones
    
    return sqrtf( 
        (d1->x - d2->x) *(d1->x - d2->x) +   //d1 et d2 pointeurs vers deux structures Drone
        (d1->y - d2->y) *(d1->y - d2->y) +
        (d1->z - d2->z) *(d1->z - d2->z) 
    );
}

//Remplissage des Drones avec des posiitons aléatoires 
void remplir_drone(int nb , Drone *debut){    //nb = nombre total des drones à remplir , et Drone *debut = pointeur vers le premier drone du tableau
   
    Drone *p = debut;   //p est un pointeur qui va se deplacer , au debut il pointe vers e meme endroit que "debut"
    Drone *fin = debut+nb;  // pointeur après le dernier drone
    int cmp = 0;  //compteur pour l'ID du drone
    
    while( p < fin){
        p->id = cmp;
        p->x = (float)(rand() % (int)(coord_max_x * precision)) / precision;   //Choisir un nbr aleatoire qui sera entre 0 et 2000 de type float 
        p->y = (float)(rand() % (int)(coord_max_y * precision)) / precision;
        p->z = (float)(rand() % (int)(coord_max_z * precision)) / precision;
        
        p++;  //passe au drone suivant
        cmp++;  //incerementation du l'ID pour le prochain
    }
}

//Detection des collisions
void detecter_collision(Drone *debut , int nb, float seuil){
    Drone *p1;
    Drone *p2;
    Drone *fin = debut+nb;
    int collisions=0;
    int paires_verifiees =0;

    for(p1= debut; p1< fin; p1++){ //parcours de chaque drone
        p2 = p1 + 1; // on commence par le drone suivant 
        //on avance tant que la difference en X est inferieur au seuil
        while (p2<fin && (p2->x - p1->x)< seuil){
            paires_verifiees++;
            
            float d = distance(p1,p2);
            if (d< seuil){
                collisions++;
                printf("\n COLLISISON N°%d , Drone #%d et Drone #%d , Distance = %.2fm \n", collisions,p1->id , p2->id, d);
            }
            p2++; //passage au drone suivant
        }
    }

    if (collisions == 0){
        printf("\n Aucune collision detectee ! \n");
    }else{
        printf("\n ATTENTION! %d collisions detectees ! \n", collisions);
    }

}

int main(){
    Drone * essaim = NULL; //pointeur vers le bloc mémoire
    printf("=====Systeme de detection de collision======");
    printf("\nNombre de drones : %d \n", NB_Drones);
    printf("Seuil de securite : %.2f m\n",Seuil_distance);

    essaim = (Drone *)malloc(NB_Drones * sizeof(Drone));

    if( essaim == NULL){
        printf("Erreur d'allocation memoire!");
        return 1;
    }

    srand((unsigned int)time(NULL)); //initialisation du générateur aléatoire
    remplir_drone(NB_Drones, essaim);
    qsort(essaim , NB_Drones, sizeof(Drone ), comparer_par_x);
    detecter_collision(essaim, NB_Drones , Seuil_distance);
    free(essaim);
    
    printf("=======Fin du programme=======");
    return 0;
}
