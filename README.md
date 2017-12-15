
#include <iostream>
#include<bits/stdc++.h>
#include<string>
#define max 50
using namespace std;
struct nodo{
    char plabra;
    struct nodo *sgte;
};
typedef struct nodo*ppila;
typedef struct nodo*tlista;
void push(ppila&,char);
char pop(ppila&);
void agregaratras(tlista&,char);
void destruir(ppila&);
int prioridadinfija(char);
int prioridadpila(char);
void imprimir(tlista&);
void balancesimbolos(ppila&,char[]);

int main(void)
{
    ppila p=NULL;
    ppila m=NULL;
    tlista lista=NULL;
    char cad[max],c,x;
    int tam;
    system("color ob");
    cout<<"conserciones de expre matematicas\n\n";
     do{
        cout<<"ingrese expresion";
         gets(cad);
        if(m!= NULL)

            destruir(m);
         balancesimbolos(m,cad);
       }
         while(m!=NULL);
         tam=strlen(cad);
         for(int i=0;i<tam;i++)
         {
             if((cad[i]>=49 &&cad[i]<=57)||(cad[i]>=97&&cad[i]<=122))
                agregaratras(lista,cad[i]);
             if(cad[i]=='+'||cad[i]=='-'||cad[i]=='*'||cad[i]=='/'||cad[i]=='('||cad[i]=='^')
             {
                 if(p==NULL)

                    push(p,cad[i]);

                 else
                 {
                    if(prioridadinfija(cad[i])>prioridadpila(p->plabra))

                        push(p,cad[i]);


                     else
                     {
                         if(prioridadinfija(cad[i])==prioridadpila(p->plabra))
                         {
                             c=pop(p);
                             agregaratras(lista,c);
                             push(p,cad[i]);
                         }
                         else
                         {
                             c=pop(p);
                             agregaratras(lista,c);
                          }
                        }
                      }
                  }


              if(cad[i]==')')
               {
                   while(p->plabra!='('&&p!=NULL)
                   {
                       c=pop(p);
                       agregaratras(lista,c);
                   }
                   if(p->plabra=='(')
                        c=pop(p);
                }

                    }
                    while(p!=NULL)
                   {
                    c=pop(p);
                      agregaratras(lista,c);
                   }
                   imprimir(lista);
                   system("pause");
                   return 0;
}
  void push(ppila&p,char a)
 {
    ppila q= new struct nodo;
    q->plabra=a;
    q->sgte=p;
    p=q;
}
char pop(ppila &p)
{
    int n;
    ppila aux;
    n=p->plabra;
    aux=p;
    p=p->sgte;
    delete(aux);
    return n;
}
void agregaratras(tlista& lista, char a)
{
    tlista t,q=new(struct nodo);
    q->plabra=a;
    q->sgte=NULL;
    if(lista==NULL)
    {
        lista= q;

    }
    else
    {
        t=lista;
        while(t->sgte!=NULL)
        {
            t=t->sgte;
        }
        t->sgte=q;
    }
}
void destruir(ppila&m)
{
    ppila aux;
    if(m!=NULL)
    {
        while(m!=NULL)
        {
            aux=m;
            m=m->sgte;
            delete(aux);
        }
    }
}
int prioridadinfija(char a)
{
    if(a=='^')
        return 4;
    if(a=='*')
        return 2;
    if(a=='/')
        return 2;
    if(a=='+')
        return 1;
    if(a=='-')
        return 1;
    if(a=='(')
        return 5;
}
int prioridadpila(char a)
{
    if(a=='^')
        return 3;
    if(a=='*')
        return 2;
    if(a=='/')
        return 2;
    if(a=='+')
        return 1;
    if(a=='-')
        return 1;
    if(a=='(')
        return 0;
}
void imprimir(tlista&lista)
{
    ppila aux;
    aux=lista;
    if(lista!=NULL)
    {
        cout<<"\t\n notacion postfija\n\n";
        while(aux!=NULL)
        {
            cout<<aux->plabra;
            aux=aux->sgte;
        }
    }
    cout<<endl<<endl;
}
void balancesimbolos(ppila&p,char cad[])
{
    ppila aux;
    int i=0;
    while(cad[i]!='\0')
    {
        if(cad[i]=='('||cad[i]=='['||cad[i]=='{')
            {
                push(p,cad[i]);
            }
            else
                if(cad[i]==')'||cad[i]==']'||cad[i]=='}')
            {
                aux=p;
                if(aux!=NULL)
                {
                    if(cad[i]==')')
                    {
                        if(aux->plabra=='(')
                         pop(p);
                    }
                    else
                        if(cad[i]==']')
                    {
                        if(aux->plabra=='[')
                            pop(p);
                    }
                    else
                        if(cad[i]=='}')
                    {
                        if(aux->plabra=='{')
                            pop(p);
                    }
                }
                else
                    push(p,cad[i]);
            }
            i++;
    }
    if(p==NULL)
        cout<<"\n\tbalanceo "<<endl<<endl;
    else
        cout<<"\n\tbalanceo incorrecto"<<endl;

}
