
# Exercise 1️⃣: File Permissions and Ownership
## 1. Create a directory called access-practice.
![Pasted image 20240622220442](https://github.com/Reemaa828/Linux_11_5/assets/112731236/77c36365-f79d-47dc-ba34-8b0bf0589029)
## 2. Inside the directory, create a file named secret.txt.
![Pasted image 20240622220805](https://github.com/Reemaa828/Linux_11_5/assets/112731236/9b87583c-87f3-4a91-b19a-2d9d6994b8b4)
## 3. Set the file permissions to allow read and write access for the owner, and no access for group and others.
![Pasted image 20240622220907](https://github.com/Reemaa828/Linux_11_5/assets/112731236/da9d2251-785a-4063-831b-432e8c3524c4)
## 4. Change the ownership of the file to a different user.
![Pasted image 20240622221048](https://github.com/Reemaa828/Linux_11_5/assets/112731236/601d1c41-4c7f-4b62-b580-7687d7152403)
## 5. Try accessing the file from the original and the different user accounts to observe the access permissions in action.
![Pasted image 20240622223810](https://github.com/Reemaa828/Linux_11_5/assets/112731236/08ca885e-1d4a-4670-ae4b-480c2de38c1c)
___

# Exercise 2️⃣: User and Group Management
## 1. Create a new user named user1.
![Pasted image 20240622224803](https://github.com/Reemaa828/Linux_11_5/assets/112731236/91a3b8b9-8f34-4e38-afef-86a9798d3357)
## 2. Create a new group named group1.
![Pasted image 20240622225007](https://github.com/Reemaa828/Linux_11_5/assets/112731236/ebcce7af-9792-4f77-8b6b-b7088e167439)
## 3. Add user1 to group1.
before

![Pasted image 20240622225657](https://github.com/Reemaa828/Linux_11_5/assets/112731236/7a590406-7d34-435e-a835-a876146e3264)

after

![Pasted image 20240622225607](https://github.com/Reemaa828/Linux_11_5/assets/112731236/90d4c07d-76e8-4d08-a5ba-b25ca1ed394c)
## 4. Change the ownership of secret.txt to user1 and group1.
![Pasted image 20240622230657](https://github.com/Reemaa828/Linux_11_5/assets/112731236/ac3ea83b-9203-4cdd-95dd-0b213d7d1cfa)
## 5. Set the file permissions to allow read and write access for the owner and the group.
![Pasted image 20240622231011](https://github.com/Reemaa828/Linux_11_5/assets/112731236/37a5f36f-5017-4587-b64f-2f4d37ffd2cf)
## 6. Test accessing the file both as user1 and a different user to understand group-based access control.
![Pasted image 20240622231131](https://github.com/Reemaa828/Linux_11_5/assets/112731236/6e2dcc85-228e-46bd-ada2-f9a49db53894)

