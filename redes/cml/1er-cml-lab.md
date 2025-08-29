# 🚧 1er CML lab

## ¿Cómo creamos un laboratorio?

## Architecture

Cisco Modeling Labs is designed and built on top of a REST-based API that exposes all the functionalities. Each operation that can be done from the Cisco Modeling Labs user interface, for example, adding nodes, creating links, or starting the simulation, is powered by the API. All other user-facing interfaces use the API to perform different operations.

You can also use that same API directly and manage the complete lab lifecycle programmatically. To help you integrate Cisco Modeling Labs in your automated workflows, there is also a Python client library that you can use to create and manage the lab simulations.

To perform the operations, the API uses the controller that is responsible for deploying virtual devices and providing all the information about the lab simulations. All the devices are deployed on available compute instances, using memory and CPU resources. The number of compute nodes depends on the type of the deployment. Cisco Modeling Labs can be deployed as a standalone solution where everything runs on the same server or virtual machine. It can also be deployed as a cluster, where multiple servers are bundled together, and the controller can use the resources on each of them.

<figure><img src="../../.gitbook/assets/image (4) (1) (1) (1).png" alt="" width="371"><figcaption><p>Tomado de https://u.cisco.com</p></figcaption></figure>

### HTML5 User Interface

Cisco Modeling Labs offers an easy-to-use HTML5 user interface that can be used to design the lab topologies and interact with the devices. It provides an intuitive graphical canvas where you can design your lab topologies with a simple drag-and-drop approach.

### REST API

All the actions that can be performed in the user interface use the underlying REST API. The API can also be used directly, meaning that anything that you can do in the user interface, you can also do programmatically. You can use your custom applications to manage the lab simulations through the API and include Cisco Modeling Labs in your NetDevOps operations.

The API is documented with a Swagger user interface, running directly on the Cisco Modeling Labs server.

## Referencias útiles

En [https://u.cisco.com/](https://u.cisco.com/for-you)  podemos encontrar un curso gratuito de **Introduction to Network Simulations with Cisco Modeling Labs** de 8 horas que nos guía en el uso de Cisco CML y desde el mismo curso en [https://ondemandelearning.cisco.com](https://ondemandelearning.cisco.com) se puede lanzar CML sin tenerlo en nuestro propio PC.

## Links

* Cursos - [https://u.cisco.com/](https://u.cisco.com/for-you)&#x20;
* Importar labs:
  * [https://github.com/CiscoDevNet/cml-community/tree/master/lab-topologies/ccna-prep](https://github.com/CiscoDevNet/cml-community/tree/master/lab-topologies/ccna-prep)
  * [https://github.com/CiscoDevNet/cml-community/tree/master/lab-topologies/cml-free/vlan-tasks](https://github.com/CiscoDevNet/cml-community/tree/master/lab-topologies/cml-free/vlan-tasks)
  * [https://www.certskills.com/category/hands-on/config-lab-2020/cml-free-lab/](https://www.certskills.com/category/hands-on/config-lab-2020/cml-free-lab/)
  *
