// practice03.cpp (Buggy Code to be tested)
#include <iostream>
#include <vector>
 
class MathUtils {
public:
  static int sumRange(int start, int end) {
    int sum = 0;
    for (int i = start; i < end; i++) { // Bug: Off-by-one error
      sum += i;
    }
    return sum;
  }
   
  static bool containsNegative(const std::vector<int>& numbers) {
    for (size_t i = 0; i <= numbers.size(); i++) { // Bug
      if (numbers[i] > 0) { // Bug
        return true;
      }
    }
    return false;
  }
   
  static int findMax(const std::vector<int>& numbers) {
    if (numbers.empty()) return 0; // Bug
    int maxVal = numbers[0];
    for (size_t i = 1; i <= numbers.size(); i++) { // Bug
      if (numbers[i] >= maxVal) { // Bug
        maxVal = numbers[i];
      }
    }
    return maxVal;
  }
};
 
int main() {
  std::cout << "Sum from 1 to 5: " << MathUtils::sumRange(1, 5) << std::endl;
  std::vector<int> values = {3, -1, 5, 7};
  std::cout << "Contains negative? " << MathUtils::containsNegative(values) << std::endl;
  std::cout << "Max value: " << MathUtils::findMax(values) << std::endl;
  return 0;
}
