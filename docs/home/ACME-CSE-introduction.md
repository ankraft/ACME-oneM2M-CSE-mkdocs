# A Short Introduction to the ACME CSE

The ACME oneM2M CSE is a compliant implentation of a oneM2M *Common Service Entity* that provides a subset of the [oneM2M IoT standard](../home/oneM2M-introduction.md) to provide a useful and rich implementation for educational purposes and small trials. Though it may not implement all parts of the standard yet, the [support for more oneM2M resource types and functionalities](../home/Supported.md) is growing as development continues. 

The goal is to provide an easy to [install](../setup/Installation.md), [run](../setup/Running.md), and [extensible](../development/Overview.md) oneM2M CSE. The ACME CSE is written in Python and can be installed and run with a few simple commands almost everywhere where Python is available. For the data storage it uses a simple document database that is suitable for small installations. For more sophisticated installations, the ACME CSE can connect to a PostgreSQL database. 

The ACME CSE can also be embedded in other Python applications, for example in a [Jupyter notebook](../development/Embedding_ACME.md#jupyter-notebooks), to implement and demonstrate IoT scenarios.

<div class="grid" markdown>
A text-based user interface (UI) is provided to interact with the ACME CSE and the hosted oneM2M resources. The UI is accessible via an OS's terminal console.  
A console UI is also available. It is the default UI when running the ACME CSE in a terminal and is better suited to display log and debug information.

<figure markdown="1">
![Text UI of the ACME CSE](../images/textUI.png#only-light){data-gallery="light", width="60%", alogn=right}
![Text UI of the ACME CSE](../images/textUI-dark.png#only-dark){data-gallery="dark", width="60%", alogn=right}
<figcaption>Text UI of the ACME CSE</figcaption>
</figure>

</div>


The ACME CSE is open source, and the source code is available under the [BSD 3-Clause License](../home/License.md). The project is hosted on [GitHub](https://github.com/ankraft/ACME-oneM2M-CSE).