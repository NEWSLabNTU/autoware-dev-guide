.PHONY: default prepare build clean

default:
	@echo 'make prepare'
	@echo '    Install required dependencies for this project.'
	@echo
	@echo 'make build'
	@echo '    Build this project.'
	@echo
	@echo 'make clean'
	@echo '    Clean up built binaries.'

prepare:
	git submodule update --init --recursive
	rosdep update --rosdistro=humble && \
	rosdep install --from-paths src --ignore-src -r

build:
	colcon build --symlink-install \
		--cmake-args -DCMAKE_BUILD_TYPE=Release \
		--cargo-args --release

clean:
	rm -rf build install log
