IBUS?=CACHED
DBUS?=CACHED
TRACE?=no
TRACE_ACCESS?=no
TRACE_START=0
ISA_TEST?=yes
MUL?=yes
DIV?=yes
CSR?=yes
MMU?=yes
ATOMIC?=no
DEBUG_PLUGIN?=STD
DEBUG_PLUGIN_EXTERNAL?=no
CUSTOM_SIMD_ADD?=no
CUSTOM_CSR?=no
DHRYSTONE=yes
FREERTOS=no
REDO?=10
REF=no
TRACE_WITH_TIME=no
REF_TIME=no
THREAD_COUNT?=4
MTIME_INSTR_FACTOR?=no


ADDCFLAGS += -CFLAGS -DIBUS_${IBUS}
ADDCFLAGS += -CFLAGS -DDBUS_${DBUS}
ADDCFLAGS += -CFLAGS -DREDO=${REDO}
ADDCFLAGS += -CFLAGS -pthread

ADDCFLAGS += -CFLAGS -DTHREAD_COUNT=${THREAD_COUNT}

ifeq ($(DHRYSTONE),yes)
	ADDCFLAGS += -CFLAGS -DDHRYSTONE
endif

ifneq ($(MTIME_INSTR_FACTOR),no)
	ADDCFLAGS += -CFLAGS -DMTIME_INSTR_FACTOR=${MTIME_INSTR_FACTOR}
endif

ifeq ($(TRACE),yes)
	VERILATOR_ARGS += --trace
	ADDCFLAGS += -CFLAGS -DTRACE
endif

ifeq ($(CSR),yes)
	ADDCFLAGS += -CFLAGS -DCSR
endif


ifeq ($(ATOMIC),yes)
	ADDCFLAGS += -CFLAGS -DATOMIC
endif

ifeq ($(CUSTOM_SIMD_ADD),yes)
	ADDCFLAGS += -CFLAGS -DCUSTOM_SIMD_ADD
endif

ifeq ($(CUSTOM_CSR),yes)
	ADDCFLAGS += -CFLAGS -DCUSTOM_CSR
endif

ifeq ($(TRACE_WITH_TIME),yes)
	ADDCFLAGS += -CFLAGS -DTRACE_WITH_TIME
endif

ifeq ($(REF_TIME),yes)
	ADDCFLAGS += -CFLAGS -DREF_TIME
endif

ifeq ($(ISA_TEST),yes)
	ADDCFLAGS += -CFLAGS -DISA_TEST
endif

ifeq ($(MMU),yes)
	ADDCFLAGS += -CFLAGS -DMMU
endif

ifeq ($(MUL),yes)
	ADDCFLAGS += -CFLAGS -DMUL
endif

ifeq ($(DIV),yes)
	ADDCFLAGS += -CFLAGS -DDIV
endif

ifeq ($(TRACE_ACCESS),yes)
	ADDCFLAGS += -CFLAGS -DTRACE_ACCESS
endif

ifneq ($(DEBUG_PLUGIN),no)
	ADDCFLAGS += -CFLAGS -DDEBUG_PLUGIN
	ADDCFLAGS += -CFLAGS -DDEBUG_PLUGIN_${DEBUG_PLUGIN}
endif

ifeq ($(DEBUG_PLUGIN_EXTERNAL),yes)
	ADDCFLAGS += -CFLAGS -DDEBUG_PLUGIN_EXTERNAL
endif

ifeq ($(REF),yes)
	ADDCFLAGS += -CFLAGS -DREF
endif

ADDCFLAGS += -CFLAGS -DTRACE_START=${TRACE_START}
ifeq ($(FREERTOS),yes)
	ADDCFLAGS += -CFLAGS -DFREERTOS
endif

all: clean run

run: compile
	./obj_dir/VVexRiscv

verilate: ../../../../VexRiscv.v
	rm -f VexRiscv.v*.bin
	cp ../../../../VexRiscv.v*.bin . | true
	verilator -cc  ../../../../VexRiscv.v  -O3 -CFLAGS -std=c++11 -LDFLAGS -pthread  ${ADDCFLAGS} --gdbbt ${VERILATOR_ARGS} -Wno-UNOPTFLAT -Wno-WIDTH --x-assign unique --exe main.cpp
 	
compile: verilate
	make  -j  -C obj_dir/ -f VVexRiscv.mk VVexRiscv
 	
clean:
	rm -rf obj_dir
	rm -f VexRiscv.v*.bin
 	
