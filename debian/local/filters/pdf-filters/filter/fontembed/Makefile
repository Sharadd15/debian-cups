SOURCES=main.c sfnt.c embed.c embed_sfnt.c test_analyse.c dynstring.c test_pdf.c fontfile.c
LIBOBJ=sfnt.o embed.o embed_sfnt.o dynstring.o fontfile.o
EXEC=ttfread test_analyze test_pdf test_ps
LIB=libfontembed.a

#CPPFLAGS=-O3 -funroll-all-loops -finline-functions -Wall -g
CPPFLAGS=-Wall -g -fPIC
LDFLAGS=

OBJECTS=$(patsubst %.c,$(PREFIX)%$(SUFFIX).o,\
        $(patsubst %.cpp,$(PREFIX)%$(SUFFIX).o,\
$(SOURCES)))
DEPENDS=$(patsubst %.c,$(PREFIX)%$(SUFFIX).d,\
        $(patsubst %.cpp,$(PREFIX)%$(SUFFIX).d,\
        $(filter-out %.o,""\
$(SOURCES))))

all: $(LIB)
test: $(EXEC)
ifneq "$(MAKECMDGOALS)" "clean"
  -include $(DEPENDS)
endif 

clean:
	rm -f $(EXEC) $(LIB) $(OBJECTS) $(DEPENDS) 

%.d: %.c
	@$(SHELL) -ec '$(CXX) -MM $(CPPFLAGS) $< \
                      | sed '\''s/\($*\)\.o[ :]*/\1.o $@ : /g'\'' > $@;\
                      [ -s $@ ] || rm -f $@'

%.d: %.cpp
	@$(SHELL) -ec '$(CXX) -MM $(CPPFLAGS) $< \
                      | sed '\''s/\($*\)\.o[ :]*/\1.o $@ : /g'\'' > $@;\
                      [ -s $@ ] || rm -f $@'

$(LIB): $(LIBOBJ)
	$(AR) rcu $@ $^ 

ttfread: main.o $(LIB)
	$(CXX) -o $@ $^ $(LDFLAGS)

test_analyze: test_analyze.o $(LIB)
	$(CXX) -o $@ $^ $(LDFLAGS)

test_pdf: test_pdf.o $(LIB)
	$(CXX) -o $@ $^ $(LDFLAGS)

test_ps: test_ps.o $(LIB)
	$(CXX) -o $@ $^ $(LDFLAGS)
