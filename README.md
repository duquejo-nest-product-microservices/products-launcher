## Local installation steps

1. Clone repository
2. Create a `.env` file using `.env.example` as template.
3. Execute `docker compose up --build -d` command.

## Steps to create Git Submodules

1. **Create a new repository on GitHub**
2. **Clone the repository to the local machine**
3. **Add the submodule**, where `repository_url` is the repository URL and `directory_name` is the name of the folder where you want the submodule to be saved (it should not already exist in the project):

   ```bash
   git submodule add <repository_url> <directory_name>
   ```

4. **Add the changes to the repository** (git add, git commit, git push). Example:

   ```bash
   git add .
   git commit -m "Add submodule"
   git push
   ```

5. **Initialize and update Submodules**: When someone clones the repository for the first time, they must run the following command to initialize and update the submodules:

   ```bash
   git submodule update --init --recursive
   ```

6. **To update the submodule references**:

   ```bash
   git submodule update --remote
   ```

### Important

If you are working in the repository that has submodules, first update and push the changes in the submodule, and then in the main repository.

If done in reverse, the submodule references in the main repository will be lost, and conflicts will need to be resolved.

---

## Useful info

**NATS**

- 4222 is for clients.
- 8222 is an HTTP management port for information reporting.
- 6222 is a routing port for clustering.

```bash
docker compose up --build -d
```

```bash
docker run -d --name nats-main -p 4222:4222 -p 6222:6222 -p 8222:8222 nats
```