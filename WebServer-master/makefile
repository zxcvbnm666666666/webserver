CXX ?= g++    #定义了一个变量CXX，如果未定义则默认为g++，表示编译器为g++。

DEBUG ?= 1    #定义了一个变量DEBUG，如果未定义则默认为1。用于设置是否开启调试模式。
ifeq ($(DEBUG), 1)     # 如果DEBUG等于1，则执行下面的语句块。
    CXXFLAGS += -g     #在CXXFLAGS变量后面追加"-g"选项，表示开启调试信息。
else
    CXXFLAGS += -O2    #在CXXFLAGS变量后面追加"-O2"选项，表示使用优化级别2。

endif

server: main.cpp  ./timer/lst_timer.cpp ./http/http_conn.cpp ./log/log.cpp ./CGImysql/sql_connection_pool.cpp  webserver.cpp config.cpp  #定义了一个目标"server"，它依赖于这些源文件。
	$(CXX) -o server  $^ $(CXXFLAGS) -lpthread -lmysqlclient    

clean:
	rm  -r server  
