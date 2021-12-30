#include <stdio.h>
#include<stdlib.h>
#include<string.h>
#include<math.h>
#define MAX 50
#define size 50
#define infnity 9999
#define none -4
typedef struct node{
  char dest_id[MAX];
  int destination_index;
  double distance;
  struct node*next;
}node;
typedef struct vertex{

  char id[MAX];
  double x,y;
  node*head;
}vertex;
typedef struct MST_E{
  int b , v ;
  double weight;

}MST_E;
MST_E mst_arr[size];
int queue[size];
int dist_[size] , path_[size] , visited_[size];
int r=-1 ,q=-1  ;
double  sum=0;
char src[100] , start[100] , end[100];

void remove_LL(vertex*graph , int n_vehicule ){
  int i;
  for(i=0 ; i<n_vehicule ; i++){
    graph[i].head=NULL;
  }
}

void init(vertex*graph){
int i;
for(i=0 ; i<  MAX ; i++){
  graph[i].head =NULL;

}
}
void read_from_file(vertex*graph , int*n_vehicules , float* range ,FILE*f)
{
  char c;
  int i=0;
  fscanf(f , "%d" , n_vehicules);
 
  c = fgetc(f);
  fscanf(f , "%f" , range);
  c= fgetc(f);
  for (i=0 ; i<*n_vehicules ; i++){
   
    fscanf(f , "%s" ,   graph[i].id);
    
    fscanf(f , "%lf" , &graph[i].x);
    fscanf(f , "%lf" , &graph[i].y);


    c=fgetc(f);
    
    
  }
  

}
node*insert(node*head , node*elem ){
node*walker=head;

  if(head==NULL){
    
    head=elem;
    return(head);
  }else if(elem->distance >head->distance){
    
    elem->next=head;
    head=elem;
    return(head);
 }else{ 

  while(walker->next!=NULL){
    if(elem->distance>walker->next->distance){
      elem->next=walker->next;
      walker->next=elem;
      return(head);
    }
    walker=walker->next;

   }
  walker->next=elem;
   }
  
  return(head);
}
double distance (double x1 , double y1 , double x2 , double y2){
  double distance = sqrt(fabs(pow(x2-x1,2)+pow(y2-y1,2)));
return distance;
}

node* create_node(int index , char*id ){
  node*element;
  element=(node*)malloc(sizeof(node));
  element->destination_index=index;
  strcpy(element->dest_id , id);
  return element;
}

void add_edge(vertex*graph  , int n_vehicule , int index_src , int index_dest , float range){
  
    double dist;
    node*elem= create_node( index_dest , graph[index_dest].id);
    
    dist=distance(graph[index_dest].x , graph[index_dest].y , graph[index_src].x , graph[index_src].y);
   
     
    if(dist > range){
        return;
    }else{
      elem->distance = dist;
      graph[index_src].head=insert(graph[index_src].head , elem);
      
    }

      elem=create_node(index_src , graph[index_src].id);
      elem->distance=dist;
      graph[index_dest].head=insert(graph[index_dest].head , elem);
}
void build_graph(vertex*graph , int n_vehicule , float range){
  int i , j;
  for(i=0 ; i<n_vehicule ; i++){
      for(j=i+1 ; j<n_vehicule ; j++){
            add_edge(graph,n_vehicule,  i, j,  range);
          }
      }
  }

void add_vehicule(vertex*graph , char*id , double x , double y , int* n_vehicules , float range){
strcpy(graph[*n_vehicules].id,id);
graph[(*n_vehicules)].x=x;
graph[(*n_vehicules)].y=y;
graph[(*n_vehicules)].head=NULL;
(*n_vehicules)++;
remove_LL(graph , *n_vehicules);
build_graph(graph , *n_vehicules , range);
}

void display(node*head , char*id_src){
  node*walker=head ;
  double temp;
  while(walker!=NULL){
   printf("(%s\t %s \t%.2f)\n",id_src ,walker->dest_id , walker->distance);
   walker=walker->next;
  }

}
void display_adj(vertex*graph , char*id , int n_vehicule){
  int i;
  for(i=0 ;i<n_vehicule ; i++){
    if(strcmp(graph[i].id , id)==0){
        display(graph[i].head , graph[i].id);
        return;
    }
    
  }
  printf("vehicule not found");
}
node*delete_from_LL(node*head , char*id){
  node*walker=head , *temp;
  if(head==NULL){
    return(head);
  
  }else if(strcmp(head->dest_id,id)==0){
          return head->next;
  }while(walker->next!=NULL){
      if(strcmp(walker->next->dest_id,id)==0){
          temp=walker->next;
          walker->next=temp->next;
          free(temp);
      return head;
    }

    walker = walker->next;
  }
      return head;

}
void display_all_edges(vertex*graph , int n_vehicule){
  int i;
    for(i=0 ;i<n_vehicule ; i++){
      display(graph[i].head,graph[i].id);
    }
}
void delete_vertex (vertex*graph , int *n_vehicule , char*id){
  int i , j;
    for(i=0 ; i<*n_vehicule ; i++){
        if(strcmp(graph[i].id , id)==0){
            graph[i].head=NULL;
              for(j=i+1 ;j<*n_vehicule ;j++){
                  graph[j-1]=graph[j];
              }
        }
    }
    for(i=0 ; i<*n_vehicule ;i++){
      graph[i].head=delete_from_LL(graph[i].head , id);
    }
    (*n_vehicule)--;
}
int find_id(char*vehicule , int n_vehicule , vertex*graph){
  int i;
  for(i=0 ; i<n_vehicule ;i++){
      if(strcmp(graph[i].id , vehicule)==0){
        return i;
      }
  }
  return 0;
}
void delete_edge(char* src , char* dest , vertex*graph , int n_vehicule){ 
  int index_src=find_id(src, n_vehicule , graph);
  int index_dest=find_id(dest, n_vehicule , graph);

  graph[index_src].head=delete_from_LL(graph[index_src].head, dest);
  graph[index_dest].head=delete_from_LL(graph[index_dest].head, src);
}

void move_vehicule(vertex*graph , char*src, int n_vehicule, double x , double y , float range){
   int index_src=find_id(src, n_vehicule , graph);
   int index_dest;
   graph[index_src].x=x;
   graph[index_src].y=y;
   remove_LL(graph , n_vehicule);
   build_graph(graph , n_vehicule, range );
  
}


void dfs (char* src , int*visited , int n_vehicule , vertex*graph){
  
  node *walker;
  
  int idx;
  
    idx=find_id(src, n_vehicule, graph);

    visited[idx]=1;
        printf("%s " ,graph[idx].id);
       
        
        
    for(walker=graph[idx].head ; walker!=NULL; walker=walker->next){

        idx=find_id(walker->dest_id, n_vehicule, graph);
         
          if(visited[idx]==0){
            
            dfs(walker->dest_id , visited , n_vehicule, graph );
            
             
          }
    }
    }
void enqueue (int m){
  if(q==-1){
        q=0;
        r=r+1;
        queue[r]=m;
    return;
  }
}
int dequeue(){
  int i;
      i=queue[q];
      q=q+1;
  return i;
}
int empty(){
  if(q>r){
    return 0;
  }else{ 
  return 1;

}
 }


void BFS(char *id , int n_vehicule , vertex *graph){
 
  int visited[50];
  int i , ind , rx , rx_ind;
  node *walker;
  
  ind = find_id( id , n_vehicule , graph);

  for (i=0 ; i<50  ;i++)
      visited[i]=0;

  printf("%s" , graph[ind].id );

      visited[ind]=1;
      enqueue(ind);
    

      
      while (empty()!=0){
        rx=dequeue();
          for(walker=graph[rx].head ; walker!=NULL ;walker=walker->next){
            rx_ind =find_id(walker->dest_id , n_vehicule , graph);
              if(visited[rx_ind]!=1){
                printf(" %s " , graph[rx_ind].id);
      
                enqueue(rx_ind);
                 
                visited[rx_ind]=1;
        }
          }
      }
  }

void mst(int n_vehicule , vertex*graph ){
  int i , j ,k, minimum , count=0 , edges=0 ;  
  int hu[size]={0}  ;
  double lenght[size] ,  least_cost=infnity ;
  node*walker;
    for(i=0 ; i<n_vehicule ; i++){
      for(walker=graph[i].head ; walker!=NULL ; walker=walker->next ){
        
          if( walker->distance < least_cost){
            k=i;
            minimum=find_id(walker->dest_id,  n_vehicule, graph);
            least_cost= walker->distance;
            
          }
      }
    }
      hu[k]=1;
      hu[minimum]=1;
      mst_arr[count].b=k;
      mst_arr[count].v=minimum;
      mst_arr[count++].weight=least_cost;
      edges++;
    
          while (edges<n_vehicule-1){
              least_cost=infnity;
              for (i=0 ; i<n_vehicule ; i++){
                  for(walker=graph[i].head ; walker!=NULL ; walker=walker->next){
                     if(walker->distance < least_cost && hu[i]==1 && hu[find_id(walker->dest_id, n_vehicule, graph)]!=1){
                        k=i;
                        minimum=find_id(walker->dest_id,  n_vehicule, graph);
                        least_cost= walker->distance;
                        
                        

                     }
                  }
              }
          
            
            hu[minimum]=1;
            mst_arr[count].b=k;
            mst_arr[count].v=minimum;
            mst_arr[count++].weight=least_cost;
            edges++;
}
}
void display_mst(int n_vehicule , vertex*graph){
  int i , j ,a ;
  MST_E o;
      for (i=0 ; i<n_vehicule-1 ;i++){ 
        
          for (j=0 ; j<n_vehicule-i-1 ; j++){ 

              if(mst_arr[j].weight >mst_arr[j+1].weight ){
                o=mst_arr[j];
                mst_arr[j]=mst_arr[j+1];
                mst_arr[j+1]=o;
                }
                }
              }
          for(a=1; a<n_vehicule ; a++){
            printf("\t%s -> %s -> %.2f\n",graph[mst_arr[a].b].id , graph[mst_arr[a].v].id, mst_arr[a].weight);
            
            
              sum+=mst_arr[a].weight;
             
          }
          }
      

void shortest_path(char* begin , char* end , int n_vehicule , vertex*graph ){
  int i , idx_begin , idx_end , idx_adj , k , minimum=9999 , idx_min ;
  node * walker1 , *walker2 , *walker3;
  idx_begin=find_id(begin, n_vehicule, graph) ;
  idx_end=find_id(end , n_vehicule , graph);
   
      for (i=0 ; i<size ; i++){
        dist_[i]=infnity;
        path_[i]=none;
        visited_[i]=0;
        
      }
      visited_[idx_begin]=1;
      dist_[idx_begin]=0;
      
  for(walker1=graph[idx_begin].head ; walker1!=NULL ; walker1=walker1->next){
    
    dist_[find_id(walker1->dest_id, n_vehicule, graph)]=walker1->distance;
    
    path_[find_id(walker1->dest_id, n_vehicule, graph)]=idx_begin;
    
    
  }
      while (visited_[idx_end]!=1){
        minimum=infnity;
          for(k=0 ; k<size ; k++)
              if(visited_[k]==0 && dist_[k]<minimum){
                minimum=dist_[k];
                idx_min=k;
               
              }
              visited_[idx_min]=1;
              for(walker2=graph[idx_min].head ; walker2!=NULL; walker2=walker2->next){
                  if( visited_[find_id(walker2->dest_id, n_vehicule, graph)]==0 &&((dist_[idx_min]+distance(graph[idx_min].x, graph[idx_min].y, graph[find_id(walker2->dest_id, n_vehicule, graph)]. x, graph[find_id(walker2->dest_id, n_vehicule, graph)].y))<dist_[find_id(walker2->dest_id , n_vehicule , graph)])){
                    

                    dist_[find_id(walker2->dest_id, n_vehicule, graph)] =(dist_[idx_min]+distance(graph[idx_min].x, graph[idx_min].y, graph[find_id(walker2->dest_id, n_vehicule, graph)]. x, graph[find_id(walker2->dest_id, n_vehicule, graph)].y));
                    path_[find_id(walker2->dest_id, n_vehicule, graph)]=idx_min;
                    
                    
                  }

              }

      }
       }
  void display_shotest_path(int begin , int end , vertex*graph){
    int q ;
    if(q==end){
      return;
      
     } else{ 
      q=path_[end];
      display_shotest_path(begin, q , graph);
       printf("\t\t\t%s -> %s %.2f\n\n" , graph[q].id , graph[end].id , distance(graph[end].x, graph[end].y, graph[q].x, graph[q].y));
       sum+=distance(graph[end].x , graph[end].y , graph[q].x , graph[q].y );
}
}
  



void menu(void){
  printf("\n\t hello dear user \n\t");
  printf("\n\t 1.  Display All Edges \n\t ");
  printf("\n\t 2.  Display Adjacent Vehicles  \n\t ");
  printf("\n\t 3.  Add a Vehicle \n\t ");
  printf("\n\t 4.  Delete a Vehicle \n\t ");
  printf("\n\t 5.  Delete an Edge \n\t ");
  printf("\n\t 6.  Move a Vehicle\n\t ");
  printf("\n\t 7.  DFS \n\t ");
  printf("\n\t 8.  BFS\n\t ");
  printf("\n\t 9. MST \n\t ");
  printf("\n\t 10. Shortest Path between 2 vehicles \n\t ");
  printf("\n\t 11.exit \n\t ");
}


int main (){
vertex graph[MAX];
int n_vehicules;
float range=0;
int choice , id , i;
FILE * f;
char * vehicule;
char v[50], v2[50];
char src[50];
f=fopen("data.txt" , "r");
init(graph);
double x, y;
int index1, index2;
int  v_dfs[100] , v_bfs[100];

read_from_file(graph, &n_vehicules, &range, f);
build_graph(graph , n_vehicules , range);


do{ 
  menu();
  printf("\tpls enter your choice : ");
  scanf("%d" , &choice);
  printf("\t\tyou have selected the choice : %d \n\n", choice);
switch(choice){
  case 1: 
    display_all_edges(graph , n_vehicules);
  break;
  case 2:
    printf("Please enter the Id of the vehicule : ");
    scanf("%s", v);
    display_adj(graph, v, n_vehicules);
  break;
  case 3:
    printf("Please enter the Id of the vehicule : ");
    scanf("%s", v);
    printf("Please enter the x cord of the vehicule : ");
    scanf("%lf", &x);
    printf("Please enter the y cord of the vehicule : ");
    scanf("%lf", &y);
    add_vehicule(graph, v, x, y, &n_vehicules, range);

  break;
  case 4:
    printf("Please enter the Id of the vehicule : ");
    scanf("%s", v);
    delete_vertex(graph,  &n_vehicules, v);
    
  break;
 
  case 5:
    printf("Please enter the Id of the vehicule : ");
    scanf("%s", v);
    printf("Please enter the Id of the vehicule : ");
    scanf("%s", v2);
    delete_edge(v, v2, graph, n_vehicules);
  break;
  case 6:
    printf("Please enter the Id of the vehicule : ");
    scanf("%s", v);
    printf("Please enter the x cord of the vehicule : ");
    scanf("%lf", &x);
    printf("Please enter the y cord of the vehicule : ");
    scanf("%lf", &y);
    move_vehicule(graph, v, n_vehicules, x, y, range);
  break;
  case 7:
  printf("\t \t pls what is the ID of the starting car ? :  ");
  scanf("%s" , src);
  if(find_id(src , n_vehicules , graph)==-1){
      printf("this imput is invalid");
  }else{
      for (i=0 ; i<size ; i++)
          v_dfs[i]=0;
        printf("\n\t\tyour car %s can reach\n\n\t\t\t" , src);
        dfs(src,v_dfs , n_vehicules , graph);
      
  }

  break;
  case 8:
  printf("\t\tpls what is the ID of the starting car ? : ");
	scanf("%s",src);
		if(find_id(src, n_vehicules, graph)==-1)
		    printf("\tthis input is invalid");
		else{
		    printf("\n\t\tReachable cars from %s are : \n",src);

		   BFS(src, n_vehicules, graph);
           
       }
  break;
  case 9:
  
  printf("\n\tOur MST is :\n\n");
  
        mst(n_vehicules  , graph);
        display_mst(n_vehicules , graph);
        printf("\t\t\t The minimum  cost is : %.2f ",sum);
  break;
  case 10:
  printf("\t \t pls what is the ID of the starting car ? ");
  scanf("%s" , start);
  printf("\n\t \t pls what is the ID of the destination car ? ");
  scanf("%s" , end);
  if(find_id(start , n_vehicules , graph)==-1|| find_id(end , n_vehicules , graph)==-1){
    printf("this imput is invalid");
  
    }else{ 
      
    shortest_path(start , end , n_vehicules , graph);
    display_shotest_path(find_id(start , n_vehicules , graph), find_id(end , n_vehicules , graph) , graph);
    printf("\t\t\t The minimum  cost is : %.2f ",sum);

  }
  
  break;
  case 11:
  printf("thank you dear user , goodbye");
  break;
  default :
  printf("This input is invalid");
  
}
}while(choice!=11);


}
