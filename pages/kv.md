- Secret - shared 
- Key: 
    - symmetric 
    - async 
- Certificate

- Tiers:
    - Standard:
        - Software
    - Premium:
        - Software + HSM (hardware<>)
        - can store keys in HSM
    - Managed HSM:
        - HSM
        - Dedicated HSM partitions (own kv)
        - can store only HSM backed keys 
        - AES

- Soft Delete
    - max 90 days
- Purge protection


- Access Policy
- RBAC

- Replicate to pair region

- Backups can be restored to the same subscription and geo location

