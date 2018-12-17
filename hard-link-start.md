ref. https://askubuntu.com/a/292819/22308

# hard link for file
```bash
mkdir -p /tmp/nn/
echo '122' > /tmp/nn/t               # t aka. test
ln           /tmp/nn/t /tmp/nn/t_hlk # t_hlk aka. hard-linked t

ls -i /tmp/nn/ # check the created i-node

cat /tmp/nn/t
cat /tmp/nn/t_hlk # :t_hlk should be identical with :t

echo '333' >> /tmp/nn/t
cat /tmp/nn/t_hlk | grep '333' #TODO why 333 not synced from :t to :t_hlk; a workaround is to use rsync manually...

rm /tmp/nn/t
ll /tmp/nn/t_hlk && cat /tmp/nn/t_hlk # :t_hlk should NOT be removed as :t
```


# hard link for folder
cannot create hard link for folder, instead we loop thru its files
```bash
mkdir -p /tmp/nn/d       # d aka. directory
mkdir -p /tmp/nn/d_hlk   # d_hlk/ aka. :d as hard-linked
echo '122' > /tmp/nn/d/t # t aka. test

for f in /tmp/nn/d/*; do 
    ln "$f" /tmp/nn/d_hlk/ 
done

ls -i /tmp/nn
ll /tmp/nn/d_hlk && cat /tmp/nn/d_hlk/t # :t_hlk should NOT be removed as :t
```
