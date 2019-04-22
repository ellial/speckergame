#include <iostream>
#include <stdexcept>
using namespace std;
class Move{
  public:
    Move(int sh, int sc, int th, int tc){
      soheap=sh;
      socoins=sc;
      tarheap=th;
      tarcoins=tc;
    };

    int getSource() const{
      return soheap;
    };
    int getSourceCoins() const{
      return socoins;
    };
    int getTarget() const{
      return tarheap;
    };
    int getTargetCoins() const{
      return tarcoins;
    };

    friend ostream & operator << (ostream &out, const Move &move){
      out<<"takes "<<move.getSourceCoins()<<" coins from heap "<<move.getSource()<<" and puts ";
      if(move.getTargetCoins()!=0) out<<move.getTargetCoins()<<" coins to heap "<<move.getTarget();
      else out<<"nothing";
      return out;
    };
  private:
    int soheap,socoins,tarheap,tarcoins;
};
class State{
  public:
    State(int h, const int c[]){
      a= new int[heaps];
      heaps=h;
      for(int i=0;i<h;i++) a[i]=c[i];
    };
    ~State(){
      delete [] a;
    };

    void next(const Move &move) throw(logic_error){
      //if kai error
      int arxthesi=move.getSource();
      int telthesi=move.getTarget();
      if(arxthesi<0||arxthesi>=heaps||telthesi<0||telthesi>=heaps)
      {
        throw logic_error("invalid heap");
      }
      else if(a[arxthesi]<(move.getSourceCoins())||(move.getTargetCoins())>=(move.getSourceCoins())){
          throw logic_error("invalid heap");
          }
          else{
          a[arxthesi]=a[arxthesi]-(move.getSourceCoins());
          a[telthesi]=a[telthesi]+(move.getTargetCoins());
          }
          };
     bool winning() const{
          for(int j=0;j<heaps;j++){
          if(a[j]!=0) return false;
          }
          return true;
          };
     int getHeaps() const{
          return heaps;
          };
     int getCoins(int h) const throw(logic_error){
          //if kai error
       if(h>=heaps||h<0) throw logic_error("Invalid heap");
       else
          return a[h];
          };

    friend  ostream & operator << (ostream &out,const State &state){
           for(int j=0;j<state.heaps-1;j++){
            out<<state.a[j]<<", ";
           }
           out<<state.a[state.heaps-1];
           return out;
          };
  private:
          int  heaps;
          int *a;
};
class Player {
  public:
    Player(const string &n){
      name=n;
    };
    virtual ~Player(){
     name.clear();
    };

    virtual const string & getType() const = 0;
    virtual Move play(const State &s) = 0;

    friend ostream & operator << (ostream &out, const Player &player){
      out<<player.getType()<<" player "<<player.name;
      return out;
    };
  protected:
    string name;

};
class GreedyPlayer: public Player{
  public:
    GreedyPlayer(const string &n): Player(n){
      type="Greedy";
    };
    virtual const string & getType() const override{
      return type;
    }
    virtual Move play(const State &s) override{
      // tarc=0;
      // th=0;
      int c,max;
      c=0;
      max=s.getCoins(0);
      for(int i=0;i<s.getHeaps()-1;i++){
        if(max<s.getCoins(i+1)){
          max=s.getCoins(i+1);
          c = i+1;
        }
      }
      // sh=c;
      // sc=max;
      return Move(c,max,0,0);
    };
  protected:
    string type;
};
class SpartanPlayer: public Player{
  public:
    SpartanPlayer(const string &n): Player(n){
      type="Spartan";
    };
    virtual const string & getType() const override{
      return type;
    };
    virtual Move play(const State &s) override{
      int maxIndex = -1;
      int maxValue = -1;

      for (int i=0; i <s.getHeaps(); i++) {
         if (s.getCoins(i) > maxValue) {
           maxIndex = i;
           maxValue = s.getCoins(i);
         }
      }


      return Move(maxIndex,1,0,0);
    };
  protected:
    string type;
};

class SneakyPlayer: public Player{
  public:
    SneakyPlayer(const string &n): Player(n){
      type="Sneaky";
    };
    virtual const string & getType() const override{
      return type;
    };
    virtual Move play(const State &s) override{
      int c,min;

      c=-1;
      min=9999999999999999; // s.getCoins(0);
      for(int i=0;i<s.getHeaps();i++){
        if(s.getCoins(i)<min && s.getCoins(i)>0){
          c=i;
          min=s.getCoins(i);
        }
      }
      return Move(c,min,0,0);
    };
  protected:
    string type;
};

class RighteousPlayer: public Player{
  public:
    RighteousPlayer(const string &n): Player(n){
      type="Righteous";
    };
    virtual const string & getType() const override{
      return type;
    };
    virtual Move play(const State &s) override{
      int c1,max,min,c2;
      c1=-1;
      c2=0;
      max=-1;
      min=s.getCoins(0);
      for(int i=0;i<s.getHeaps();i++){
        if(max<s.getCoins(i)){
          c1=i;
          max=s.getCoins(i);
        }

        if(s.getCoins(i)<min){
          c2=i;
          min=s.getCoins(i);
        }
      }
if(max%2!=0) max=(max)+1;
if(min%2!=0) min=(min)+1;

      return Move(c1,max/2,c2,(max/2)-1);
    };
  protected:
    string type;
};
class Game{
  public:
    Game(int heaps,int players){
      p = players;
      h = heaps;
      a = new int [h];
      seira = new Player* [p];//gia to play kyklika
      HeapCount = 0;
      PlayerCount = 0;
    };
    ~Game(){
      delete[] a;
      delete[] seira;
    }

    void addHeap(int coins) throw(logic_error){
      if( HeapCount> h-1 || coins<0)
       throw logic_error("invalid");

      else a[HeapCount++]=coins;
    };
    void addPlayer(Player *player) throw(logic_error){
      //elegxos gia error
      if(PlayerCount>p-1) throw logic_error("invalid");
      else seira[PlayerCount++]=player;
    };
    void play(ostream &out) throw(logic_error){
      //playercount==p error

      if(PlayerCount!=p &&  HeapCount!=h){
        throw logic_error("invalid");
        return ;
      }
      int i = 0;
      State current(h,a);
      while(!current.winning()){
        out<<"State: "<<current<<endl;
        out<<*seira[i % p]<<" "<<seira[i % p]->play(current)<<endl;
        current.next(seira[i % p]->play(current));
          i++;

      }
      out<<"State: "<<current<<endl;
      i--;
      out<<*seira[i % p]<<" wins"<<endl;
    };
  private:
    int *a,p,h,PlayerCount,HeapCount;
    Player **seira;
};
