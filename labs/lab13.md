## LAB 13: Route Table

![rt-diagram](../assets/rt.png)

### Langkah 1: Edit Route

1. Click "Route Table" anda dalam "Resource Map" VPC: `my-vpc`
2. Click "Edit Route" dan "Add Route"
3. Letakkan destination: `0.0.0.0/0` dan target `Internet Gateway`
4. Pilih `my-igw` sebagai target dan Click "Save changes"

### Langkah 2: Sahkan di VPC Resource Map

1. Click "Your VPC" dan pilih VPC anda: `my-vpc`
2. Rujuk "Resource Map" dan lihat jika ada sambungan ke Internet Gateway
3. Anda akan dapati tiada kerana kita belum setup Route Table
