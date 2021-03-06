if(APPLE)
  ADD_OSQUERY_OBJCXX_LIBRARY(FALSE osquery_tables_objc
    ../core/darwin/NSProcessInfo+PECocoaBackports.mm
    ../core/darwin/NSProcessInfo+PECocoaBackports.h
    ../core/darwin/PECocoaBackportsGlobal.h
    system/darwin/osx_version.mm
    system/darwin/users.mm
    system/darwin/groups.mm
    system/darwin/ca_certs.mm
  )

  ADD_OSQUERY_LIBRARY(FALSE osquery_tables_darwin
    events/darwin/passwd_changes.cpp
    events/darwin/hardware_events.cpp
    networking/darwin/routes.cpp
    system/darwin/acpi_tables.cpp
    system/darwin/apps.cpp
    system/darwin/block_devices.cpp
    system/darwin/ca_certs_utils.cpp
    system/darwin/firewall.h
    system/darwin/firewall.cpp
    system/darwin/homebrew_packages.cpp
    system/darwin/iokit_registry.cpp
    system/darwin/iokit_utils.cpp
    system/darwin/kernel_info.cpp
    system/darwin/kernel_extensions.cpp
    system/darwin/launchd.cpp
    system/darwin/mounts.cpp
    system/darwin/nvram.cpp
    system/darwin/preferences.cpp
    system/darwin/processes.cpp
    system/darwin/process_open_descriptors.cpp
    system/darwin/quarantine.cpp
    system/darwin/pci_devices.cpp
    system/darwin/usb_devices.cpp
    system/darwin/smbios_tables.cpp
    system/darwin/startup_items.cpp
    system/darwin/xprotect.cpp
    system/darwin/nfs_shares.cpp
    system/darwin/xattr.cpp
  )

  ADD_OSQUERY_LINK(FALSE "-framework CoreFoundation")
  ADD_OSQUERY_LINK(FALSE "-framework Security")
  ADD_OSQUERY_LINK(FALSE "-framework OpenDirectory")
  ADD_OSQUERY_LINK(FALSE "-framework DiskArbitration")
elseif(FREEBSD)
  ADD_OSQUERY_LIBRARY(FALSE osquery_tables_freebsd
    events/freebsd/passwd_changes.cpp
    networking/freebsd/routes.cpp
    system/freebsd/processes.cpp
    system/freebsd/users.cpp
    system/freebsd/groups.cpp
  )
else()
  ADD_OSQUERY_LIBRARY(FALSE osquery_tables_linux
    events/linux/hardware_events.cpp
    events/linux/passwd_changes.cpp
    networking/linux/arp_cache.cpp
    networking/linux/process_open_sockets.cpp
    networking/linux/routes.cpp
    system/linux/acpi_tables.cpp
    system/linux/block_devices.cpp
    system/linux/groups.cpp
    system/linux/kernel_info.cpp
    system/linux/kernel_integrity.cpp
    system/linux/kernel_modules.cpp
    system/linux/memory_map.cpp
    system/linux/mounts.cpp
    system/linux/pci_devices.cpp
    system/linux/processes.cpp
    system/linux/process_open_files.cpp
    system/linux/shared_memory.cpp
    system/linux/smbios_tables.cpp
    system/linux/usb_devices.cpp
    system/linux/users.cpp
  )

  if(CENTOS)
    # CentOS specific tables
    ADD_OSQUERY_LIBRARY(FALSE osquery_tables_redhat
      system/linux/rpm_packages.cpp
    )

    ADD_OSQUERY_LINK(FALSE "rpm")
    ADD_OSQUERY_LINK(FALSE "rpmio")
  elseif(UBUNTU)
    # Ubuntu specific tables
    ADD_OSQUERY_LIBRARY(FALSE osquery_tables_ubuntu
      system/linux/deb_packages.cpp
      system/linux/apt_sources.cpp
    )

    ADD_OSQUERY_LINK(FALSE "apt-pkg")
    ADD_OSQUERY_LINK(FALSE "dpkg")
  endif()

  ADD_OSQUERY_LINK(FALSE "procps" "proc")
  ADD_OSQUERY_LINK(FALSE "blkid")
  ADD_OSQUERY_LINK(FALSE "udev")
  ADD_OSQUERY_LINK(FALSE "uuid")
endif()

ADD_OSQUERY_LIBRARY(FALSE osquery_tables
  networking/etc_hosts.cpp
  networking/etc_services.cpp
  networking/interfaces.cpp
  networking/listening_ports.cpp
  networking/utils.cpp
  system/cpuid.cpp
  system/crontab.cpp
  system/last.cpp
  system/shell_history.cpp
  system/smbios_utils.cpp
  system/suid_bin.cpp
  system/logged_in_users.cpp
)

ADD_OSQUERY_LIBRARY(TRUE osquery_utility_tables
  utility/time.cpp
  utility/hash.cpp
  utility/file.cpp
  utility/osquery.cpp
)

ADD_OSQUERY_TEST(FALSE etc_hosts_tests networking/etc_hosts_tests.cpp)
if(APPLE)
  ADD_OSQUERY_TEST(FALSE xattr_tests system/darwin/xattr_tests.cpp)
  ADD_OSQUERY_TEST(FALSE apps_tests system/darwin/apps_tests.cpp)
  ADD_OSQUERY_TEST(FALSE ca_certs_tests system/darwin/ca_certs_tests.cpp)
  ADD_OSQUERY_TEST(FALSE firewall_tests system/darwin/firewall_tests.cpp)
  ADD_OSQUERY_TEST(FALSE launchd_tests system/darwin/launchd_tests.cpp)
endif()
