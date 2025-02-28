#include <iostream>
#include <cmath> 
#include <iomanip>
using namespace std;
struct HalfFloat {
  unsigned int mantissa : 10; 
  unsigned int exponent : 5;  
  unsigned int sign : 1;      
};

float halfToFloat(unsigned short half) {
  HalfFloat hf;
  memcpy(&hf, &half, sizeof(hf));
  int sign = hf.sign;
  int exponent = hf.exponent;
  int mantissa = hf.mantissa;
  if (exponent == 0) {
    if (mantissa == 0) {
      return (sign == 0) ? 0.0f : -0.0f;
    } else {
      return 0.0f; 
    }
  } else if (exponent == 31) {
    if (mantissa == 0) {
      return (sign == 0) ? INFINITY : -INFINITY;
    } else {
      return NAN;
    }
  }
  float normalizedMantissa = 1.0f + (float)mantissa / 1024.0f; 
  float floatExponent = pow(2.0f, (float)(exponent - 15)); 
  float result = (sign == 0 ? 1.0f : -1.0f) * normalizedMantissa * floatExponent;
  return result;
}
int main() {
  unsigned short halfFloatValue;
  cout << "Введите 16-битное число с плавающей точкой в шестнадцатеричном виде (например, 4400): ";
  cin >> hex >> halfFloatValue;
  float floatValue = halfToFloat(halfFloatValue);
  cout << "Десятичное значение: " << fixed << setprecision(10) << floatValue << endl;
  return 0;
}
