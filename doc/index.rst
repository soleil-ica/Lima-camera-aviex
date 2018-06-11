.. _camera-aviex:

Aviex camera plugin
-------------------

.. image:: aviex_front.png

.. image:: aviex_back.png

Intoduction
```````````
The PCCD-170170 is a large area detector (4096 x 4096) designed for use in WAXS or SAXS experiments in a vacuum environment.

.. image:: aviex_inside.png

.. image:: aviex_block_diagram.png

The detector supports full frame, multiframe time-sliced, and streak camera modes of operation.

Used at the SWING beamline of Synchrotron SOLEIL to make timeresolved SAXS measurements together with another WAXS detector.

This Lima plugin controls an Aviex camera under linux.

It is based on the `MX beamline control <http://mx.iit.edu>`_ toolkit.

It has been tested at the Synchrotron SOLEIL facility, but has not been installed yet on a Beamline.

Module configuration
````````````````````

First, compile the Mx Library/Driverand and install it in the default path (``/opt/mx/``).

Start the Mx driver with:

.. code-block:: sh

	cd /opt/mx/sbin/
	./mx start

Then, follow the generic instructions in :ref:`build_installation`. If using CMake directly, add the following flag:

.. code-block:: sh

 -DLIMACAMERA_AVIEX=true

For the Tango server installation, refers to :ref:`tango_installation`.

Initialisation and Capabilities
```````````````````````````````

Implementing a new plugin for new detector is driven by the LIMA framework but the developer has some freedoms to choose which standard and specific features will be made available. This section is supposed to give you the correct information regarding how the camera is exported within the LIMA framework.

Camera initialisation
.....................

There are 2 parameters to be filled with your Lima client:

	- The detector friendly name: can be any string defined by user.
	- The detector database file name: this file must contains configuration parameters such as IP adress, port.

Std capabilites
...............

This plugin has been implemented in respect of the mandatory capabilites but with some limitations according to some programmer's choices. We only provide here extra information for a better understanding of the capabilities for the Aviex camera.

* HwDetInfo

 - Max image size is : 4096 * 4096
 - 16 bit unsigned type is supported

* HwSync trigger type supported are:

	- IntTrig
	- ExtTrigSingle

Optional capabilites
........................

* HwBin

	- 1 * 1
	- 2 * 2
	- 4 * 4
	- 8 * 8
	- Binning above are typical values, but binning is not necessarily square.

* HwRoi

	Not yet implemented

Configuration
`````````````

No specific hardware configuration is needed.

How to use
````````````

Here is the list of accessible fonctions to configure and use the Aviex detector:

.. code-block:: cpp

	//-- Related to Aviex specific features
	void getExpMultiplier(double& exp_mult);
	void setExpMultiplier(double exp_mult);
	void getLatencyTime(double& period_time);
	void setLatencyTime(double  period_time);
	void getGapMultiplier(double& gap_mult);
	void setGapMultiplier(double  gap_mult);
	void getMxLibraryVersion(std::string& version);
	void getInternalAcqMode(std::string& acq_mode);
	//! Available mode : ONESHOT, MULTIFRAME, GEOMETRICAL, MEASURE_DARK, MEASURE_FLOOD_FIELD
	void setInternalAcqMode(const std::string& mode);
	void getReadoutDelayTime(double& readout_delay);
	void setReadoutDelayTime(double readout_delay);
	void getReadoutSpeed(bool& readout_speed);
	void setReadoutSpeed(bool readout_speed);
	void getInitialDelayTime(double& initial_delay);
	void setInitialDelayTime(double initial_delay);
	//! MASK_CORRECTION_BIT_POSITION		= 0
	//! BIAS_CORRECTION_BIT_POSITION		= 1
	//! DARK_CORRECTION_BIT_POSITION		= 2
	//! FLOOD_CORRECTION_BIT_POSITION		= 3
	//! GEOM_CORRECTION_BIT_POSITION		= 12
	void setCorrectionFlags(unsigned long);
