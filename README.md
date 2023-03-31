Ansible Role: Nebulon Manage Volumes
=========

Manages volumes for Nebulon nPods.

Requirements
------------

- NebPyClient 2.0.8+
- Nebulon Ansible Module 1.4.0+

Role Variables
--------------

    # State for all volumes
    volume_state: absent

    # Specify LUN export method
    # (present|all) -> All nPod servers can access export
    # (host) -> Make export available to a single host. Requires host_uuid
    # (local) -> Make export available only to the local host that owns the volume
    # (absent) -> Remove the volume export
    export_type: absent

    # nPod name to use when managing volumes/exports
    npod_name: "K8s_Lenovo"

    host_uuid:

    # List of volumes to manage (create or remove)
    volumes:
      - name: "server-09-local-kadalu"
        size: 1000000000000
        mirrored: True
        owner_spu_serial: 012386435A3944xxxx
        backup_spu_serial: 01231C5BA68435xxxx
        state: "{{ volume_state }}"

      - name: "server-10-local-kadalu"
        size: 1000000000000
        mirrored: True
        owner_spu_serial: 01231C5BA68435xxxx
        backup_spu_serial: 012386435A3944xxxx
        state: "{{ volume_state }}"

      - name: "server-11-local-kadalu"
        size: 1000000000000
        mirrored: True
        owner_spu_serial: 01234C1DDA9533xxxx
        backup_spu_serial: 01236AE13059FDxxxx
        state: "{{ volume_state }}"

      - name: "server-12-local-kadalu"
        size: 1000000000000
        mirrored: True
        owner_spu_serial: 01236AE13059FDxxxx
        backup_spu_serial: 01234C1DDA9533xxxx
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
    # ansible-playbook -i inventory/lenovo.yml playbooks/nebulon_volume/manage_nebulon_volumes.yml

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

      roles:
        - { role: ansible-role-nebulon-manage-volumes }

License
-------

MIT

Author Information
------------------

Aaron Patten
aaronpatten@gmail.com
