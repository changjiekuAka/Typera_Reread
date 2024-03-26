```
#include <iostream>
#include <functional>
#include <unordered_map>

void doBack() { std::cout << "Back" << std::endl; }
void doRun() {std::cout << "Run" << std::endl; }
void doExit() {std::cout << "Exit" << std::endl; }
void doShow() {std::cout << "Show" << std::endl; }

int main()
{
	unordered_map< int , std::function<void()> > actionMap;
	actionMap.insert({1,doBack});
	actionMap.insert({2,doRun});
	actionMap.insert({3,doExit});
	actionMap.insert({4,doShow});
	
	// 替换使用switch 
	int choose;
	std::cin >> choose;
	auto it = actionMap.find(choose);
	it->second();
	
	return 0;
}
```

