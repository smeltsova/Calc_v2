CXX = g++
CXXFLAGS = -Wall -Werror -Wextra -std=c++17
TEST = test/*.cc
GTEST_DIR = test
EXPRESSION = model/expression.cc
CREDITCALCULATOR = model/creditcalculator.cc
OS := $(shell uname)

ifeq ($(OS),Darwin)
TESTLIBS = -lm -lpthread -fprofile-arcs -ftest-coverage
else
TESTLIBS = -lgtest -lpthread -lm  -lrt -lsubunit -fprofile-arcs -ftest-coverage
endif

all: clean install open
.PHONY : all

install: clean
	mkdir build && cd build && cmake  ../CMakeLists.txt && make
ifeq ($(OS),Darwin)
	mv build/calc.app ~/Desktop
else
	mv build/calc ~/Desktop
endif
	rm -rf build/
.PHONY : install


uninstall:
ifeq ($(OS),Darwin)
	rm -rf ~/Desktop/calc.app
else
	rm -rf ~/Desktop/calc
endif
.PHONY : uninstall

open:
ifeq ($(OS),Darwin)
	open -n ~/Desktop/calc.app --args -AppCommandLineArg
else
	~/Desktop/calc
endif
.PHONY : open	

calculator.o: $(EXPRESSION) $(CREDITCALCULATOR)
	@$(CXX) $(CXXFLAGS) -c $(EXPRESSION) $(CREDITCALCULATOR)
.PHONY : calculator.o

calculator.a: calculator.o
	@ar rcs calculator.a *.o
.PHONY : calculator.a

test: clean calculator.a 
	@$(CXX) $(CXXFLAGS) $(TEST) calculator.a -o unit_test -lgtest -lpthread
	@./unit_test
.PHONY : test

gcov_report: clean calculator.o
ifeq ($(OS),Darwin)
	$(CXX) $(CXXFLAGS) -lgtest $(CREDITCALCULATOR) $(EXPRESSION) $(TEST) $(TESTLIBS)  -o  gcov_test 
else
	$(CXX) $(CXXFLAGS) $(CREDITCALCULATOR) $(EXPRESSION) $(TEST) -o gcov_test $(TESTLIBS)
endif
	@chmod +x * 
	./gcov_test
	lcov -t "gcov_test" -o gcov_test.info --no-external -c -d .
	genhtml -o report/ gcov_test.info
ifeq ($(OS),Darwin)
	open ./report/index.html
else
	xdg-open ./report/index.html
endif
.PHONY : gcov_report

dist:
	mkdir SmartCalc/
	mkdir SmartCalc/src
	cp -r controller model test view Makefile CMakeLists.txt README.pdf SmartCalc/src
	tar -cvzf SmartCalc.tar.gz SmartCalc
	rm -rf SmartCalc
.PHONY : dist

format:
	clang-format -style=Google -i */*.cc  */*.h
.PHONY : format

check: 
	clang-format -style=Google -n */*.cc  */*.h  */*.cpp --verbose
.PHONY : check

clean:
	@rm -rf *.o *.a *.cmake CMakeCache.txt
	@rm -rf *.gcda *.gcno *.info
	@rm -rf gcov_report
	@rm -rf gcov_test
	@rm -rf report
	@rm -rf unit_test
	@rm -rf build/
	@rm -rf Archive_calc
	@rm -rf dist-newstyle/
.PHONY : clean
