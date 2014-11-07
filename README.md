CST-150-202
===========

basic programs

#ifndef CarList_h
#define CarList_h

#include "Standards.h"
#include "CarData.h"
class CarList
{
private:
	CarData cars[35];
	int numberOfCars;

public:
	int GetNumberOfCars(void);

	void SetCars(ifstream& fin);

	void UpdateBasePrice(string aManufacturer, double aPercent);

	void OutputNumberOfCars(ofstream& fout);
	void OutputCars(ofstream& fout);
	void OutputDivider(ofstream& fout);
	void OutputListByManufacturer(ofstream& fout, string aManufacturer);
	void OutputListToHighPrice(ofstream& fout, double aBasePrice);
	void OutputListByCarClass(ofstream& fout);
};

#endif



#include "CarData.h"
#include "CarList.h"
#include "Standards.h"

int CarList::GetNumberOfCars(void)
{
	return numberOfCars;
}

void CarList::SetCars(ifstream& fin)
{
	int index;
	int carIndex;
	int tempMpgArray[2];
	string tempManufacturer;
	string tempModel;
	double tempBasePrice;
	string tempCarClass;

	carIndex = 0;

	getline(fin, tempManufacturer, ' ');

	while (fin && carIndex < 35)
	{
		fin >> numberOfCars;

		for (index = 0; index < numberOfCars; index++)
		{
			cars[carIndex].SetManufacturer(tempManufacturer);

			getline(fin, tempModel, '#');
			cars[carIndex].SetModel(tempModel);

			fin >> tempBasePrice;
			cars[carIndex].SetBasePrice(tempBasePrice);

			fin >> tempMpgArray[0];
			fin >> tempMpgArray[1];
			cars[carIndex].SetMpg(tempMpgArray);

			fin >> tempCarClass;
			
			//getline(fin, tempCarClass, '\n');
			cars[carIndex].SetCarClass(tempCarClass);

			carIndex++;
		}

		fin.ignore(100, '\n');
		getline(fin, tempManufacturer, ' ');
	}
	numberOfCars = carIndex;
}

void CarList::OutputListByManufacturer(ofstream& fout, string aManufacturer)
{
	int index;
	int number;
	number = 1;
	int tempArray[2];
	fout << "List By Manufacturer -" << aManufacturer << endl;
	for (index = 0; index < numberOfCars; index++)
	{
		if (cars[index].GetManufacturer() == aManufacturer)
		{
			fout << number << setw(20) << cars[index].GetCarClass() << setw(20) <<
				cars[index].GetBasePrice() << endl;
			number++;
		}
	}

	 
}

void CarList::OutputNumberOfCars(ofstream& fout)
{
	fout << "The number of cars are :" << numberOfCars << endl;
}

void CarList::OutputCars(ofstream& fout)
{
	CarData outputCars;
	int index;
	for (index = 0; index < numberOfCars; index++)
	{
		outputCars.OutputCarData(fout);
	}
}

void CarList::OutputListToHighPrice(ofstream& fout, double aBasePrice)
{
	int index;
	int number;
	number = 1;
	int tempArray[2];
	fout << "List Of Cars under a price or " << aBasePrice << endl;
	for (index = 0; index < numberOfCars; index++)
	{
		if (cars[index].GetBasePrice() < aBasePrice)
		{
			fout << number << setw(20) << cars[index].GetCarClass() << setw(20) <<
				cars[index].GetBasePrice() << endl;
			number++;

		}
	}


}

void CarList::UpdateBasePrice(string aManufacturer, double aPercent)
{
	int index;
	double tempBasePrice;

	aPercent = aPercent / 100;

	for (index = 0; index < numberOfCars; index++)
	{
		if (aManufacturer == cars[index].GetManufacturer())
		{
			tempBasePrice = cars[index].GetBasePrice() + (cars[index].GetBasePrice() * aPercent);
			cars[index].SetBasePrice(tempBasePrice);
		}
	}
}
