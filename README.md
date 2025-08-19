# Assembler_project
authors: Basel Swaid
assembler overview

    this assembler is a custom tool designed to translate assembly language code into machine code for a hypothetical cpu 
    it involves several key steps including preprocessing first and second passes and the generation of output files 
    each step is crucial for ensuring that the assembly code is correctly parsed analyzed and converted into executable machine code

detailed breakdown of key files

	preprocessor.h and preprocessor.c

    these files handle the preprocessing step which is the first stage in the assembly process 
    preprocessing involves tasks such as expanding macros and removing comments here’s how it works

		macro expansion
		
		    macros are templates or shorthand notations that are expanded into their full form during preprocessing 
		    for example a macro might define a sequence of instructions that can be reused multiple times in the code 
		    during preprocessing each instance of a macro call is replaced with the corresponding sequence of instructions
		    the macro definitions are collected and the macro names are stored in an array while the content of the macro is stored in another array
		    
		removing comments
		
		    comments are removed from the code as they do not translate into machine instructions 
		    the preprocessor identifies lines or portions of lines that are comments and excludes them from further processing
		    
		handling directives
		
		    certain directives like .extern and .entry are handled during preprocessing 
		    these directives provide instructions to the assembler about external symbols or entry points in the code 
		    the preprocessor records these for use in later stages of assembly

	first_pass.h and first_pass.c

    the first pass is responsible for collecting labels and building the initial structure of the program 
    during this pass the assembler goes through the code line by line and processes labels instructions and directives
    
		label handling
		
		    labels are identifiers used to mark locations in the code 
		    during the first pass each label is recorded along with its address 
		    this allows the assembler to resolve references to these labels later in the process 
		    
		instruction analysis
		
		    each instruction is analyzed to determine how many machine code lines it will require 
		    the addressing modes of the operands are checked and the necessary space is allocated in the instruction counter (ic) 
		    errors such as missing operands or invalid addressing modes are caught during this pass
		    
		data and string handling
		
		    the .data and .string directives are handled during the first pass 
		    these directives define data elements and strings that will be stored in memory 
		    the first pass calculates how much space these elements will require and adjusts the data counter (dc) accordingly

	second_pass.h and second_pass.c

    the second pass involves generating the actual machine code based on the information gathered during the first pass 
    this pass focuses on resolving label references encoding instructions and finalizing the code and data images
    
		code generation
		
		    the main task of the second pass is to generate machine code for each instruction 
		    this involves converting the assembly instructions into binary representations that the cpu can execute
		    
		label resolution
		
		    during this pass any references to labels are resolved to their actual addresses 
		    if a label was defined in the code its address is used in place of the label name in the generated machine code 
		    if a label was not found an error is generated
		    
		handling externals and entries
		
		    the second pass also finalizes the handling of external symbols and entry points 
		    any references to external symbols are recorded and the necessary entries are made in the extern and entry files

	file_writer.h and file_writer.c

    the file writer is responsible for generating the final output files that represent the assembled program 
    these files typically include the object file (.ob) the extern file (.ext) and the entry file (.ent)
    
		writing the object file
		
		    the object file contains the machine code generated during the second pass 
		    it includes both the code image and the data image in a format that can be loaded into memory for execution
		    
		writing extern and entry files
		
		    the extern file lists all external symbols used in the program along with the locations where they are referenced 
		    the entry file lists all the entry points into the program defined by the .entry directive

	definitions.h

    this file contains all the constants and macro definitions that are used throughout the assembler 
    it includes values like the initial instruction counter (ic) data counter (dc) and the maximum lengths for labels and lines 
    this file centralizes all the key constants to make the code more maintainable and readable

	utils.h and utils.c

    these files contain utility functions that are used across different parts of the assembler 
    functions such as string manipulation error checking and memory management are implemented here 
    these utilities help keep the main assembly logic clean and focused on the core tasks

	important structs

		MacroArray
		
		    stores macros defined during preprocessing includes an array of macro names and their associated content
		    
		LabelArray
		
		    stores labels encountered during the first pass includes label names and their corresponding addresses
		    
		ExternEntryArray
		
		    stores external symbols and entry points includes the names of externals and entries and the lines where they are used
		    
		MachineWordsArray
		
		    stores the binary machine code words generated during the second pass includes the actual binary data and additional metadata

	important methods

		preprocess_file
		
		    handles the preprocessing of the input file expands macros removes comments and prepares the code for the first pass
		    
		first_pass
		
		    processes the assembly code to collect labels calculate instruction and data sizes and catch early errors
		    
		second_pass
		
		    generates the final machine code resolves label references and prepares the output for writing to files
		    
		write_ob_file
		
		    writes the final object file containing the assembled machine code
		    
		write_entries_to_file
		
		    writes the entry file listing all entry points in the code
		    
		write_externs_to_file
		
		    writes the extern file listing all external symbols and their references

	how it works

	the assembler operates in multiple stages to ensure the correct translation of assembly code into machine code
		
		preprocessing
		
		    the assembler first preprocesses the input assembly file during this stage macros are expanded comments are removed and directives like .extern and .entry are recorded
		
		first pass
		
		    during the first pass the assembler scans the preprocessed file collecting labels and calculating the space needed for instructions and data
			it also checks for syntax errors and invalid operations
		
		second pass
		
		    in the second pass the assembler generates the actual machine code it resolves any label references converts instructions into binary and handles externals and entries
		
		writing output files
		
		    after the second pass the assembler writes the machine code to an object file it also generates extern and entry files
			if needed these files can then be used to load the program into a simulator or run it on the hypothetical cpu

	how to operate

    to use the assembler you would typically run it from the command line passing the names of one or more assembly files as arguments 
    the assembler will process each file in turn generating the necessary output files in the same directory

    - NOTE: TO RUN THE EXAMPLES TYPE: ./assembler inputs_and_outputs/input inputs_and_outputs/input2

		example usage
		
		    ./assembler file1 file2
		    
		in this example the assembler will process file1.as and file2.as generating file1.ob and the file1.ext file1.ent (if necessary) and similarly for file2
