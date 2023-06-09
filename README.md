# mount-tmpfs
GitHub action to create and mount a temporary file system in memory.

It can be used as an attempt to keep secrets from being written to disk.

## Usage

> :warning: Please consider the physical limitations of the GitHub runners before
            changing the values.

```yaml
    - name: Get a tmpfs for our secret
      id: tmpfs
      uses: LeastAuthority/mount-tmpfs-action@v1
      with:
        size: 2
        root: '/mnt'
```

The action then returns the uuid and the mount point of the tmpfs as outputs.

```yaml
    - name: Import secret in tmpfs
      run: |
        cat <<EOF > "${{ steps.tmpfs.outputs.mnt }}/secret_key"
        ${{ secrets.KEY }}
        EOF
```

Optionally, the tmpfs could be removed when no longer needed.

```yaml
    - name: Cleanup
      run: |
        sudo umount "${{ steps.tmpfs.outputs.mnt }}"
```
