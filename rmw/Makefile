SHELL = /bin/zsh
PWD_DIR = "$(shell basename $$(pwd))"
BUILD_DIR = build

# ==================================
# Pushes the current directory to remote host.
# ==================================

NUM_PROCS = 4
REMOTE_USER_HOST = "patrick@vm_comp4961_ubuntu1804"
REMOTE_DEST_DIR = "~/remote/$(shell hostname -s)/"

.PHONY: push-remote
push-remote:
	# Make the directory on the remote if it doesn't exist already.
	(ssh -t $(REMOTE_USER_HOST) "mkdir -p $(REMOTE_DEST_DIR)$(PWD_DIR)")
	# Sync our current directory with the remote.
	(rsync -a \
 			--delete \
 			--exclude "build" \
 			--exclude "install" \
 			--exclude "log" \
 			--exclude "build-remote" \
 			--exclude "cmake-build*" \
 			--exclude ".vscode" \
 			--exclude ".idea" \
 			--exclude ".git" \
 			--exclude ".gitignore" \
 			./ $(REMOTE_USER_HOST):$(REMOTE_DEST_DIR)$(PWD_DIR))

# ==================================
# Runs a Make command remotely.
# ==================================

.PHONY: remote
remote: push-remote
	ssh -t $(REMOTE_USER_HOST) "\
		cd $(REMOTE_DEST_DIR)$(PWD_DIR) ; \
		zsh -ilc 'make $(MAKE_CMD)' ; "

# ==================================
# Clean
# ==================================

.PHONY: clean
clean:
	rm -rf $(BUILD_DIR)

# ==================================
# Build
# ==================================

.PHONY: build
build:
	CC=aarch64-none-elf-gcc CXX=aarch64-none-elf-g++ \
		cmake -S . -B $(BUILD_DIR) \
		-DBUILD_TESTING=0 \
		-DCMAKE_C_COMPILER_WORKS=1 \
		-DCMAKE_CXX_COMPILER_WORKS=1 \
		-DCMAKE_MODULE_PATH=./
	cd $(BUILD_DIR) && $(MAKE) -j $(NUM_PROCS)
	# Install so that we can use the library in other projects.
	cd $(BUILD_DIR) && sudo $(MAKE) install
	#Install the project...
	#-- Install configuration: ""
	#-- Installing: /usr/local/share/rmw/cmake
	#-- Installing: /usr/local/share/rmw/cmake/get_rmw_typesupport.cmake
	#-- Installing: /usr/local/share/rmw/cmake/configure_rmw_library.cmake
	#-- Installing: /usr/local/share/rmw/cmake/register_rmw_implementation.cmake
	#-- Installing: /usr/local/include/rmw
	#-- Installing: /usr/local/include/rmw/rmw
	#-- Installing: /usr/local/include/rmw/rmw/validate_namespace.h
	#-- Installing: /usr/local/include/rmw/rmw/validate_node_name.h
	#-- Installing: /usr/local/include/rmw/rmw/rmw.h
	#-- Installing: /usr/local/include/rmw/rmw/get_node_info_and_types.h
	#-- Installing: /usr/local/include/rmw/rmw/events_statuses
	#-- Installing: /usr/local/include/rmw/rmw/events_statuses/events_statuses.h
	#-- Installing: /usr/local/include/rmw/rmw/events_statuses/liveliness_changed.h
	#-- Installing: /usr/local/include/rmw/rmw/events_statuses/offered_deadline_missed.h
	#-- Installing: /usr/local/include/rmw/rmw/events_statuses/liveliness_lost.h
	#-- Installing: /usr/local/include/rmw/rmw/events_statuses/incompatible_qos.h
	#-- Installing: /usr/local/include/rmw/rmw/events_statuses/message_lost.h
	#-- Installing: /usr/local/include/rmw/rmw/events_statuses/requested_deadline_missed.h
	#-- Installing: /usr/local/include/rmw/rmw/validate_full_topic_name.h
	#-- Installing: /usr/local/include/rmw/rmw/init_options.h
	#-- Installing: /usr/local/include/rmw/rmw/publisher_options.h
	#-- Installing: /usr/local/include/rmw/rmw/get_topic_endpoint_info.h
	#-- Installing: /usr/local/include/rmw/rmw/impl
	#-- Installing: /usr/local/include/rmw/rmw/impl/cpp
	#-- Installing: /usr/local/include/rmw/rmw/impl/cpp/key_value.hpp
	#-- Installing: /usr/local/include/rmw/rmw/impl/cpp/macros.hpp
	#-- Installing: /usr/local/include/rmw/rmw/impl/cpp/demangle.hpp
	#-- Installing: /usr/local/include/rmw/rmw/impl/config.h
	#-- Installing: /usr/local/include/rmw/rmw/domain_id.h
	#-- Installing: /usr/local/include/rmw/rmw/event.h
	#-- Installing: /usr/local/include/rmw/rmw/init.h
	#-- Installing: /usr/local/include/rmw/rmw/get_service_names_and_types.h
	#-- Installing: /usr/local/include/rmw/rmw/features.h
	#-- Installing: /usr/local/include/rmw/rmw/security_options.h
	#-- Installing: /usr/local/include/rmw/rmw/ret_types.h
	#-- Installing: /usr/local/include/rmw/rmw/serialized_message.h
	#-- Installing: /usr/local/include/rmw/rmw/event_callback_type.h
	#-- Installing: /usr/local/include/rmw/rmw/qos_profiles.h
	#-- Installing: /usr/local/include/rmw/rmw/convert_rcutils_ret_to_rmw_ret.h
	#-- Installing: /usr/local/include/rmw/rmw/qos_string_conversions.h
	#-- Installing: /usr/local/include/rmw/rmw/incompatible_qos_events_statuses.h
	#-- Installing: /usr/local/include/rmw/rmw/message_sequence.h
	#-- Installing: /usr/local/include/rmw/rmw/get_topic_names_and_types.h
	#-- Installing: /usr/local/include/rmw/rmw/topic_endpoint_info.h
	#-- Installing: /usr/local/include/rmw/rmw/qos_policy_kind.h
	#-- Installing: /usr/local/include/rmw/rmw/subscription_content_filter_options.h
	#-- Installing: /usr/local/include/rmw/rmw/types.h
	#-- Installing: /usr/local/include/rmw/rmw/localhost.h
	#-- Installing: /usr/local/include/rmw/rmw/time.h
	#-- Installing: /usr/local/include/rmw/rmw/network_flow_endpoint.h
	#-- Installing: /usr/local/include/rmw/rmw/macros.h
	#-- Installing: /usr/local/include/rmw/rmw/subscription_options.h
	#-- Installing: /usr/local/include/rmw/rmw/sanity_checks.h
	#-- Installing: /usr/local/include/rmw/rmw/error_handling.h
	#-- Installing: /usr/local/include/rmw/rmw/topic_endpoint_info_array.h
	#-- Installing: /usr/local/include/rmw/rmw/network_flow_endpoint_array.h
	#-- Installing: /usr/local/include/rmw/rmw/allocators.h
	#-- Installing: /usr/local/include/rmw/rmw/visibility_control.h
	#-- Installing: /usr/local/include/rmw/rmw/get_network_flow_endpoints.h
	#-- Installing: /usr/local/include/rmw/rmw/names_and_types.h
	#-- Installing: /usr/local/lib/librmw.a