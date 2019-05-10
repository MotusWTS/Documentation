# System Design Documentation

This section will cover both design concepts and development tasks. Each subsection will cover a separate sub-project of the Motus system.

## find_tags

The find_tags program is a C++ command line (CLI) program that actually identifies tags within a given input recording. The source files are located in the github repository [https://github.com/MotusWTS/find_tags](https://github.com/MotusWTS/find_tags).

### Development Environment

In order to build find_tags, you will need to have make and gcc installed. Also, you will need to have the development packages for boost and sqlite installed. This is easiest in a Linux environment.
