CXX = g++
CXXFLAGS = -Wall -Werror -Wextra -std=c++17
TARGET:= ./Application/SmartCalc.app
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
	mkdir -p build && mkdir -p Application \
	&& cd build && cmake -DEXECUTABLE_OUTPUT_PATH="../Application" ../ && cmake --build . 
	rm -rf build/
.PHONY : install

uninstall:
	rm -rf Application
.PHONY : uninstall

open:
	open $(TARGET)
.PHONY : open	

calculator.o: $(EXPRESSION) $(CREDITCALCULATOR)
	@$(CXX) $(CXXFLAGS) -c $(EXPRESSION) $(CREDITCALCULATOR)

test: clean calculator.o
	@$(CXX) $(CXXFLAGS) $(TEST) calculator.o -o unit_test -lgtest -lpthread
	@./unit_test
.PHONY : test

clean:
	@rm -rf *.o *.a *.cmake CMakeCache.txt
	@rm -rf *.gcda *.gcno *.info
	@rm -rf gcov_report
	@rm -rf gcov_test
	@rm -rf report
	@rm -rf unit_test
	@rm -rf build/
.PHONY : clean
