# InteligentAdaptativeFilter
Adaptative Filter for Any Objetive
This is an adaptative filter, or a linear predictive model with machine learning, in the most easy way
#We need to
-> Define the class
-> Give their data
-> Train 
-> And Run
# The magic are:
Instrucctions
- What number of elements do you want?  # define BackFilter 10
- What will your Learning parameter? # define LearningParameter 0.3

## The class that makes the magic
class FiltroInteligente{

	//We have three variables with all the information
	float Vin[BackFilter]; //Input values
	float Kca[BackFilter]; //K parameters
	float Pro[BackFilter]; //=inputs * K parameters
	Method to clean the vectors
	void ClearData(){
		for(int i=0; i<BackFilter;i++)	{
			Vin[i]=0;
			Pro[i]=0;
		}
	}
	Limpieza del vector de datos
	void ClearProd(){
		for(int i=0; i<BackFilter;i++)	{
			Pro[i]=0;
		}
	}
	//Funcion para predecir los datos
	public: 
	float Result=0;
	float Wantted=0;
	float ErrorN=0;
	float ErrorS=0;
	//Randomize K
	void RandomizeK(){
		for(int i=0; i<BackFilter;i++)	{
			Kca[i]=rand()%BackFilter;
	 	}
	}
	// Make the operacion of Result=sum(Vin*Kpar) Remeber that Vin and Kpar are vectors
	void ProductoYPrecedir(){
		Result=0;
		for(int i=0; i<BackFilter;i++)	{
			Pro[i]=Vin[i]*Kca[i];
			Result=Pro[i]+Result;
	 	}
	}
	//Initialize the class
	void Initialize(){
		ClearData();
		RandomizeK();
		Vin[BackFilter-1]=1; //La ultima posicion es el desface llamada Beta
	}
	//Print my data of learning
	void PrintMySavedData(){
		printf("Saved:\t");
		for(int i=0; i<BackFilter;i++)	{
			printf("%3.3f\t",Vin[i]);
	 	}
	 	printf("\nK:\t");
		for(int i=0; i<BackFilter;i++)	{
			printf("%3.3f\t",Kca[i]);
	 	}
	}
	//Insert an X value, remember the last position are 1 because
	/*
	y=mx+b
	Where K are a vector that represents m
	And Vin are a vector that represents x
	But the b can be absorbed with
	[x1,x2,x3,...] point [k1,k2,k3] +b => [x1,x2,x3,..,1] point [k1,k2,k3,b]
	*/
	void AddX(float Add){
		for(int i=0; i<(BackFilter-1);i++)	{
			Vin[i]=Vin[i+1];
	 	}
	 	Vin[BackFilter-2]=Add;
	}
	//This traing the model One Time
	/*Using 
		h(x)=mx+b
		Where 
		e=1/2*(y-h(x))^2
		de/dk=(y-h(x))*(Vin) //Remeber that y-h(x) is a number that will be multiplied by a vector
		//(y-h(x))*(Vin) => This will be a vector
		k(n)=k(n-1)+de/dk =>Remeber that k is a vector and de/dk also
	*/
	void TraingOnce(float WanttedData){
		ProductoYPrecedir();
		ErrorN=WanttedData-Result;
		ErrorS=1/2*(ErrorN)*ErrorN;
		for(int i=0; i<BackFilter;i++)	{
			Kca[i]=Kca[i]+ErrorN*Vin[i]*0.1;
	 	}
	}
};
