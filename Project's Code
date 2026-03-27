#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#define SIZE 10
#define MAX 20

// ----------SUSPECT MODULE (HASH TABLE) ----------
// Suspect structure
typedef struct Suspect{
    int id;
    char name[50];
    struct Suspect*next;
    }Suspect;
Suspect*hashTable[SIZE];

// Hash function
int hashFunction(int id)
   {
    return id%SIZE;
   }

// Create suspect node
Suspect*createSuspectNode(int id,char name[])
  {
    Suspect*newNode=(Suspect*)malloc(sizeof(Suspect));
    newNode->id=id;
    strcpy(newNode->name,name);
    newNode->next=NULL;
    return newNode;
  }

// Find suspect
Suspect*findSuspect(int id)
  {
    int index=hashFunction(id);
    Suspect*temp=hashTable[index];
    while(temp!=NULL){
        if(temp->id==id)
            return temp;
        temp=temp->next;
    }
    return NULL;
  }

// Add suspect
void addSuspect()
  {
    int id;
    char name[50];
    printf("Enter Suspect ID: ");
    scanf("%d",&id);

    if(findSuspect(id)!=NULL)
      {
        printf("Duplicate ID not allowed\n");
        return;
      }

    printf("Enter Suspect Name: ");
    scanf("%s",name);

    Suspect*newNode=createSuspectNode(id,name);

    int index=hashFunction(id);

    newNode->next=hashTable[index];
    hashTable[index]=newNode;

    printf("Suspect added successfully\n");
  }

// Show all suspects
void showAllSuspects()
  {
    printf("\n--- Suspect List ---\n");

    for(int i=0;i<SIZE;i++)
    {
        Suspect*temp=hashTable[i];
        while(temp!=NULL)
        {
          printf("ID: %d  Name: %s\n",temp->id,temp->name);
          temp=temp->next;
        }
    }
  }

// ---------------- CASE MODULE (BST) ----------------
// Case structure
typedef struct CaseNode{
    int caseID;
    int suspectID;
    char title[50];
    struct CaseNode*left;
    struct CaseNode*right;
    }CaseNode;
       CaseNode*root=NULL;

// Create case node
CaseNode*createCaseNode(int id,int sid,char title[])
   {

    CaseNode*newNode=(CaseNode*)malloc(sizeof(CaseNode));
    newNode->caseID=id;
    newNode->suspectID=sid;
    strcpy(newNode->title,title);
    newNode->left=NULL;
    newNode->right=NULL;
    return newNode;
  }

// Insert case into BST
CaseNode*insertCaseNode(CaseNode*root,int id,int sid,char title[])
   {
    if(root==NULL)
        return createCaseNode(id,sid,title);

    if(id<root->caseID)
        root->left=insertCaseNode(root->left,id,sid,title);
    else
        root->right=insertCaseNode(root->right,id,sid,title);
    return root;
  }

// Add case
void addCase()
  {
    int id,sid;
    char title[50];

    printf("Enter Case ID: ");
    scanf("%d",&id);

    printf("Enter Suspect ID: ");
    scanf("%d",&sid);

    printf("Enter Case Title: ");
    scanf("%s",title);

    root=insertCaseNode(root,id,sid,title);

    printf("Case added successfully\n");
  }

// Search case
void searchCase(CaseNode*root,int id)
  {
    if(root==NULL)
     {
        printf("Case not found\n");
        return;
     }

    if(root->caseID==id){
        printf("Case Found\n");
        printf("Title: %s  Suspect ID: %d\n",root->title,root->suspectID);
        return;
    }

    if(id<root->caseID)
        searchCase(root->left,id);
    else
        searchCase(root->right,id);
  }

// Show all cases (sorted)
void showAllCases(CaseNode*root)
   {
    if(root==NULL)
        return;

    showAllCases(root->left);
    printf("CaseID: %d  Title: %s  SuspectID: %d\n",
           root->caseID,root->title,root->suspectID);
    showAllCases(root->right);
  }

// ---------------- GRAPH MODULE ----------------
int graph[MAX][MAX];
int graphIds[MAX];
int graphCount=0;

// Register node
int registerGraphId(int id)
  {
    for(int i=0;i<graphCount;i++)
        {
         if(graphIds[i]==id)
            return i;
        }

    graphIds[graphCount]=id;
    graphCount++;

    return graphCount-1;
  }

// Link suspects
void linkSuspects(){

    int s1,s2;

    printf("Enter Suspect 1 ID: ");
    scanf("%d",&s1);

    printf("Enter Suspect 2 ID: ");
    scanf("%d",&s2);

    int i=registerGraphId(s1);
    int j=registerGraphId(s2);

    graph[i][j]=1;
    graph[j][i]=1;

    printf("Relationship added\n");
}

// Show graph
void showGraph()
  {
    printf("\nRelationship Matrix:\n");
    for(int i=0;i<graphCount;i++)
      {
        for(int j=0;j<graphCount;j++)
            printf("%d ",graph[i][j]);

        printf("\n");
     }
  }

// ---------------- FILE HANDLING ----------------
// Save suspects
void saveSuspects()
   {
    FILE*fp=fopen("suspects.txt","w");

    for(int i=0;i<SIZE;i++)
      {
        Suspect*temp=hashTable[i];

        while(temp!=NULL)
          {
            fprintf(fp,"%d %s\n",temp->id,temp->name);
            temp=temp->next;
          }
      }
    fclose(fp);
   }

// Load suspects
void loadSuspects()
  {
    FILE*fp=fopen("suspects.txt","r");
    if(fp==NULL)
        return;

    int id;
    char name[50];

    while(fscanf(fp,"%d %s",&id,name)!=EOF)
     {

        Suspect*newNode=createSuspectNode(id,name);

        int index=hashFunction(id);

        newNode->next=hashTable[index];
        hashTable[index]=newNode;
      }
    fclose(fp);
  }

// ---------------- MENU SYSTEM ----------------
void menu(){
    int choice,id;
    while(1){
        printf("\n--- Crime Record Management System ---\n");

        printf("1. Add Suspect\n");
        printf("2. Show All Suspects\n");
        printf("3. Add Case\n");
        printf("4. Show All Cases\n");
        printf("5. Search Case\n");
        printf("6. Link Suspects\n");
        printf("7. Show Relationship Graph\n");
        printf("8. Exit\n");

        printf("Enter choice: ");
        scanf("%d",&choice);

        switch(choice){

            case 1:
                addSuspect();
                break;

            case 2:
                showAllSuspects();
                break;

            case 3:
                addCase();
                break;

            case 4:
                showAllCases(root);
                break;

            case 5:
                printf("Enter Case ID to search: ");
                scanf("%d",&id);
                searchCase(root,id);
                break;

            case 6:
                linkSuspects();
                break;

            case 7:
                showGraph();
                break;

            case 8:
                saveSuspects();
                printf("Data saved. Exiting program.\n");
                return;

            default:
                printf("Invalid choice\n");
        }
    }
}

// ---------------- MAIN FUNCTION ----------------
int main(){
    loadSuspects();
    menu();
    return 0;
}
