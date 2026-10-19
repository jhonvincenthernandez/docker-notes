# Student Grade App — Docker Build & Push Guide

## Image naming format

General format:

```text
{software-name}:{tag-version}
```

Example:

```text
student-grade-app:v1
```

When pushing to Docker Hub, add your Docker Hub username as the namespace:

```text
{dockerhub-username}/{software-name}:{tag-version}
jhonvincent/student-grade-app:v1
```

| Part | Meaning |
| --- | --- |
| `software-name` | Name of your app/image, e.g. `student-grade-app`. |
| `tag-version` | Version of the image, e.g. `v1`. |
| `dockerhub-username` | Your Docker Hub account name, e.g. `jhonvincent`. |

> `student-grade-app:v1` is enough for local use.
> Use `jhonvincent/student-grade-app:v1` when pushing to Docker Hub.

---

## Prerequisites

Before running the commands, make sure:

- Docker Desktop is installed and running.
- You have a Docker Hub account (`jhonvincent`).
- A `Dockerfile` exists in this folder.
- Your terminal is opened inside this folder (`docker notes`).

---

## Step 1 — Build the Docker image

Choose one naming option:

```bash
# Option 1 — Local use only: {software-name}:{tag-version}
docker build -t student-grade-app:v1 .

# Option 2 — For Docker Hub push: {dockerhub-username}/{software-name}:{tag-version}
docker build -t jhonvincent/student-grade-app:v1 .
```

What the parts mean:

| Part | Meaning |
| --- | --- |
| `docker build` | Builds a Docker image from a Dockerfile. |
| `-t student-grade-app:v1` | Tags (names) the image. Format: `{software-name}:{tag-version}`. |
| `-t jhonvincent/student-grade-app:v1` | Full Docker Hub tag. Format: `{dockerhub-username}/{software-name}:{tag-version}`. |
| `.` | Uses the current folder as the build context. |

> If you already built `student-grade-app:v1`, retag it before pushing:
>
> ```bash
> # Add the Docker Hub username namespace to an existing image
> docker tag student-grade-app:v1 jhonvincent/student-grade-app:v1
> ```

---

## Step 2 — Log in to Docker Hub

```bash
# Log in to Docker Hub (enter your username and password/token when prompted)
docker login
```

- Username: `jhonvincent`
- Password: your Docker Hub password or access token.

---

## Step 3 — Push the image to Docker Hub

```bash
# Upload the local image to Docker Hub
docker push jhonvincent/student-grade-app:v1
```

> The repository `jhonvincent/student-grade-app` must exist on Docker Hub,
> or Docker Hub will create it automatically on the first push (depending on your account settings).
>
> The pushed name must match your local image tag. A local-only name like
> `student-grade-app:v1` cannot be pushed directly — it needs the
> `jhonvincent/` namespace first (use `docker tag` as shown in Step 1).

---

## Step 4 — Verify your local images

```bash
# List all Docker images stored on your machine
docker images
```

Check that `jhonvincent/student-grade-app` with tag `v1` appears in the list.

---

## Step 5 — Clean up unused Docker data

```bash
# Remove stopped containers, unused networks, dangling images, and build cache
docker system prune
```

Type `y` and press **Enter** when asked to confirm.

### Optional cleanup (more aggressive)

```bash
# Also remove all unused images (not just dangling ones)
docker system prune -a

# Also remove unused volumes — WARNING: this deletes volume data permanently
docker system prune -a --volumes
```

> `docker system prune` does **not** remove images used by running containers.
> Review the list it shows before confirming.

---

## Complete command sequence

Copy and run these in order:

```bash
# 1. Build the image from the Dockerfile in the current folder
docker build -t jhonvincent/student-grade-app:v1 .

# 2. Log in to Docker Hub
docker login

# 3. Push the image to Docker Hub
docker push jhonvincent/student-grade-app:v1

# 4. List local images to confirm
docker images

# 5. Clean up unused Docker data
docker system prune
```

---

## Optional — Run the image locally

```bash
# Run the image in a container (adjust the port mapping to match your app)
docker run --rm -p 3000:3000 jhonvincent/student-grade-app:v1
```

- `--rm` deletes the container automatically when it stops.
- `-p 3000:3000` maps port `3000` on your machine to port `3000` inside the container.
  Change these numbers if your app uses a different port.

---

## Common issues

| Problem | Fix |
| --- | --- |
| `docker: command not found` | Start Docker Desktop and reopen the terminal. |
| `failed to read dockerfile` | Make sure `Dockerfile` is in the current folder. |
| `denied: requested access to the resource is denied` | Run `docker login` again and check the image name uses your Docker Hub username. |
| `unauthorized` during push | Your session expired — log in again with `docker login`. |
| Image does not appear after push | Confirm the tag: it must be `jhonvincent/student-grade-app:v1`. |
