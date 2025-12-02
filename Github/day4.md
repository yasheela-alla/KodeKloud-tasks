<img width="1268" height="745" alt="Screenshot 2025-12-02 182308" src="https://github.com/user-attachments/assets/33266181-d92d-4218-8697-ea585d1fc6fe" />


# ✅ **Step-by-Step Solution**

## **1. SSH into the jump host**

You’re already on the jump host usually, but if not:

```bash
ssh thor@jump_host
# password: mjolnir123
```

## **2. Copy the sample index.html to the storage server**

Use **scp** as user `natasha` (as required by all storage tasks):

```bash
scp /tmp/index.html natasha@ststor01:/usr/src/kodekloudrepos/blog/
```

Enter password: `Bl@kW`

This puts the file in the correct folder inside the cloned repo.

## **3. SSH into the Storage Server**

```bash
ssh natasha@ststor01
# password: Bl@kW
```

## **4. Go to the repo directory**

```bash
cd /usr/src/kodekloudrepos/blog
```

## **5. Add the file to Git**

```bash
git add index.html
```

## **6. Commit the file**

```bash
git commit -m "Add sample index.html"
```

## **7. Push the changes**

Usually the repo is a *bare repo* located at `/opt/blog.git`, and the clone has the `origin` remote already set.

So simply:

```bash
git push origin master
```

If master is the default branch, this works.

<img width="966" height="862" alt="Screenshot 2025-12-02 183200" src="https://github.com/user-attachments/assets/ea569f6c-c2ab-4357-8d1b-4d956c7aeadf" />


# 🎉 **Done! Expected State**

* `/usr/src/kodekloudrepos/blog/index.html` exists
* Commit added
* Repo pushed to `/opt/blog.git`
* No permission or ownership changes required

<img width="983" height="629" alt="image" src="https://github.com/user-attachments/assets/27d6d438-c08b-4f64-8008-0f09d5aab08a" />
