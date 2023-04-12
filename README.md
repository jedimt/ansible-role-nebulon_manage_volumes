Ansible Role: Nebulon Manage Volumes
=========

Manages volumes for Nebulon nPods.

Requirements
------------

- NebPyClient 2.0.8+
- Nebulon Ansible Module 1.4.0+

Role Variables
--------------

Variables are defined in the defaults/main.yml file. In this example, the SPU serial numbers are stored in an ansible vault as well.

    # State for all volumes (present|absent)
    volume_state: present

    # Specify LUN export method (does not affect underlying volume)
    # (present|all) -> All nPod servers can access export
    # (host) -> Make export available to a single host. Requires host_uuid
    # (local) -> Make export available only to the local host that owns the volume
    # (absent) -> Remove the volume export
    export_type: local

    # nPod name to use when managing volumes/exports
    npod_name: "K8s_Lenovo"

    host_uuid:

    # List of volumes to manage (create or remove)
    volumes:
      - name: "server-10-local-kadalu"
        size: 1000000000000
        mirrored: true
        owner_spu_serial: "{{ server-10.spu-serial }}"
        backup_spu_serial: "{{ server-09-spu-serial }}"
        state: "{{ volume_state }}"

      - name: "server-11-local-kadalu"
        size: 1000000000000
        mirrored: true
        owner_spu_serial: "{{ server-11-spu-serial }}"
        backup_spu_serial: "{{server-12-spu-serial }}"
        state: "{{ volume_state }}"

      - name: "server-12-local-kadalu"
        size: 1000000000000
        mirrored: true
        owner_spu_serial: "{{server-12-spu-serial }}"
        backup_spu_serial: "{{ server-11-spu-serial }}"
        state: "{{ volume_state }}"

Dependencies
------------

None.

Example Playbook
----------------

    # ===========================================================================
    # Manage Nebulon Volumes
    # ===========================================================================

    # Example invocation:
    # ansible-playbook -i inventory/lenovo.yml playbooks/ansible-playbook-nebulon-volume/manage_nebulon_volumes.yml

    - name: Manage Nebulon Volumes
      hosts: localhost
      connection: local
      tags: play_neb_vols
      gather_facts: false

      # module_defaults requires nebulon.nebulon_on version 1.2.1 or later
      module_defaults:
        group/nebulon.nebulon_on.nebulon:
          neb_username: "{{ vault_neb_username }}"
          neb_password: "{{ vault_neb_password }}"

      vars_files:
        # Ansible vault with all required passwords
        - "../../credentials.yml"
        # Ansible vault with server and SPU serials
        - "../../serials.yml"

      roles:
        - { role: jedimt.nebulon_manage_volumes }

License
-------

MIT

Author Information
------------------

Aaron Patten
aaronpatten@gmail.com
