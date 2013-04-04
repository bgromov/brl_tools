brl_tools
=========

A collection of tools/scripts for [ROS](http://ros.org).

recompose.py
------------

Reads data from `stdin` as a YAML-document and generate another YAML-document according to given config.

**Usage**:

    recompose <config>

**Parameters**:

    config  --  YAML-document where values of elements are paths to elements of input YAML-document.

**Examples**:

 * To generate a list (array) out of some dictionary:

    echo -e "{path: {to: {object: {field_X: 0.1, field_Y: 0.2, field_Z: 0.3}}}}\n---\n" > test.yaml
    cat test.yaml | ./recompose.py "[path/to/object/field_X, path/to/object/field_Y, path/to/object/field_Z]"

 * To generate a dictionary out of some list (array):

    echo -e "{path: {to: {object: [0.1, 0.2, 0.3]}}}\n---\n" > test.yaml
    cat test.yaml | ./recompose.py "{point: {x: 'path/to/object[0]', y: 'path/to/object[1]', z: 'path/to/object[2]'}}"

 *Note the quotes embracing the paths to array elements*

 * To use with `rostopic` in ROS:

    rostopic echo /gazebo_bumper | recompose.py "{header: header, wrench: 'states[0]/total_wrench'}" | rostopic pub -r 10 /wrench geometry_msgs/WrenchStamped

