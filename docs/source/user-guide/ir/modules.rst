=======
Modules
=======

.. currentmodule:: llvmlite.ir

A module is a compilation unit. It defines a set of related 
functions, global variables and metadata. In the IR layer, a 
module is represented by the :class:`Module` class.


.. class:: Module(name='')

   Create a module. For informational purposes, you can specify 
   the optional *name*, a Python string.

   Modules have the following methods and attributes:

   * .. method:: add_debug_info(kind, operands, is_distinct=False)

        Add debug information metadata to the module with the 
        given *operands*---a mapping of string keys to values---or 
        return a previous equivalent metadata. *kind* is the name 
        of the debug information kind.

        EXAMPLE: ``'DICompileUnit'``

        A :class:`DIValue` instance is returned. You can then 
        associate it to, for example, an instruction.

        EXAMPLE::

           di_file = module.add_debug_info("DIFile", {
                "filename": "factorial.py",
                "directory": "bar",
           })
           di_compile_unit = module.add_debug_info("DICompileUnit", {
                "language": ir.DIToken("DW_LANG_Python"),
                "file": di_file,
                "producer": "llvmlite x.y",
                "runtimeVersion": 2,
                "isOptimized": False,
           }, is_distinct=True)

   * .. method:: add_global(globalvalue)

        Add the given *globalvalue*---a :class:`GlobalValue`---to 
        this module. It should have a unique name in the whole 
        module.

   * .. method:: add_metadata(operands)

        Add an unnamed :ref:`metadata` node to the module with 
        the given *operands*---a list of metadata-compatible 
        values. If another metadata node with equal operands 
        already exists in the module, it is reused instead. 
        Returns an :class:`MDValue`.

   * .. method:: add_named_metadata(name, element=None)

        Return the metadata node with the given *name*. If it does 
        not already exist, the named metadata node is created 
        first. If *element* is not ``None``, it can be a metadata 
        value or a sequence of values to append to the metadata 
        node's elements. Returns a :class:`NamedMetaData`.

        EXAMPLE::

           module.add_named_metadata("llvm.ident", ["llvmlite/1.0"])

   * .. method:: get_global(name)

        Get the 
        :ref:`global value`---a :class:`GlobalValue`---with the 
        given name. :exc:`KeyError` is raised if the name does 
        not exist.

   * .. method:: get_named_metadata(name)

        Return the metadata node with the given *name*. 
        :exc:`KeyError` is raised if the name does not exist.

   * .. method:: get_unique_name(name)

        Return a unique name across the whole module. *name* is 
        the desired name, but a variation can be returned if it 
        is already in use.

   * .. attribute:: data_layout

        A string representing the data layout in LLVM format.

   * .. attribute:: functions

        The list of functions, as :class:`Function` instances, 
        declared or defined in the module.

   * .. attribute:: global_values

        An iterable of global values in this module.

   * .. attribute:: triple

        A string representing the target architecture in LLVM 
        "triple" form.
