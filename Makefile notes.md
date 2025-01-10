In Makefiles, a phony target is not a real file. It’s a label for a task that needs to be executed. Marking a target as .PHONY tells Make to run the associated commands every time, regardless of whether a file with the same name exists.

When a target is not phony, Make checks if the corresponding file already exists. If it exists and its timestamp is newer than its prerequisites, Make will skip rebuilding that target.

By marking linux/arch/arm/boot/zImage as phony, the Makefile forces the build process to run every time, regardless of whether the file already exists. This ensures that the kernel is always recompiled, which is important in cases where:

The configuration might have changed.
The toolchain might have been updated.
Dependencies might not be properly tracked by Make.

Simply put, a file labelled as phony means this file is critical.


% is a wildcard that matches any filename

| build: An order-only dependency that ensures the build/ directory exists before the target is created.
