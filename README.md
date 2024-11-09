# Persistence ✨
Persistence adalah cara untuk menyimpan data secara permanen ke dalam sebuah database atau sistem penyimpanan lainnya, dimana data akan tetap ada meskipun program dihentikan atau komputer dimatikan. Nah, di dalam java, cara kerja persistence ini didefinisikan dengan JPA atau Java Persistence API.

## Langkah-langkah membuat persistence 🚀
### 1. 💻 Klik kanan pada package yang telah dibuat, kemudian pilih entity classes from databases

![p1](https://github.com/user-attachments/assets/915a8969-470a-4d65-800f-f7a44314d2e7)

### 2. 👾 Pilih database connection

![p2](https://github.com/user-attachments/assets/aaa33cbc-e18f-46a4-91bb-ac1e848170b2)

### 3. 📂➡️ Add tabel yang dibutuhkan, kemudian klik next sampai finish

![p3](https://github.com/user-attachments/assets/2b1b7a89-abb6-41d2-ae8c-4d638a91a6bc)

![p4](https://github.com/user-attachments/assets/de3883dd-67b3-4549-9ef9-d664cba4b060)

![p5](https://github.com/user-attachments/assets/97684298-555a-4212-b2cd-786a94ddb363)

### 4. 💥 Maka secara otomatis akan terdapat package baru, yaitu META-INF yang berisi persistence.xml, juga akan terdapat class baru yaitu Matakuliah_1.java

![p6](https://github.com/user-attachments/assets/2e859039-76e7-49bb-a1e6-1fe5389c8f1d)

### 5. 📥 Source code untuk button INSERT

Persistence unit name saya adalah:

![p7](https://github.com/user-attachments/assets/e49fcf74-1a6b-431e-93d5-d0dfa3cf2571)

    private void btnInsertActionPerformed(java.awt.event.ActionEvent evt) {                                          
        EntityManagerFactory manfact = Persistence.createEntityManagerFactory("PersistancePU");
        EntityManager man = manfact.createEntityManager();

        try {
            man.getTransaction().begin();
            Matakuliah_1 matkul = new Matakuliah_1();
            matkul.setKodemk(txtKode.getText());
            matkul.setNamamk(txtNama.getText());
            matkul.setSks(Integer.parseInt(txtSKS.getText()));
            matkul.setSemesterajar(Integer.parseInt(txtSem.getText()));

            man.persist(matkul);
            man.getTransaction().commit();

            JOptionPane.showMessageDialog(null, "Data berhasil ditambahkan");
            tampil();
        } catch (Exception e) {
            man.getTransaction().rollback();
            JOptionPane.showMessageDialog(null, "Input Data Gagal: " + e.getMessage());
        } finally {
            man.close();
            manfact.close();
        }
    } 

### 6. 🔄 Source code untuk button UPDATE

    private void btnUpdateActionPerformed(java.awt.event.ActionEvent evt) {                                          
        EntityManagerFactory manfact = Persistence.createEntityManagerFactory("PersistancePU");
        EntityManager man = manfact.createEntityManager();

        try {
            man.getTransaction().begin();
            Matakuliah_1 matkul = man.find(Matakuliah_1.class, txtKode.getText());

            if (matkul != null) {
                matkul.setNamamk(txtNama.getText());
                matkul.setSks(Integer.parseInt(txtSKS.getText()));
                matkul.setSemesterajar(Integer.parseInt(txtSem.getText()));

                man.getTransaction().commit();
                JOptionPane.showMessageDialog(null, "Data berhasil diupdate");
                tampil();
            } else {
                JOptionPane.showMessageDialog(null, "Data tidak ditemukan");
                man.getTransaction().rollback();
            }
        } catch (Exception e) {
            man.getTransaction().rollback();
            JOptionPane.showMessageDialog(null, "Update Data Gagal: " + e.getMessage());
        } finally {
            man.close();
            manfact.close();
        }
        this.kode_auto();
    }

### 7. 🗑️ Source code untuk button DELETE

     private void btnDeleteActionPerformed(java.awt.event.ActionEvent evt) {                                          
        EntityManagerFactory manfact = Persistence.createEntityManagerFactory("PersistancePU");
        EntityManager man = manfact.createEntityManager();

        try {
            man.getTransaction().begin();
            Matakuliah_1 matkul = man.find(Matakuliah_1.class, txtKode.getText());

            if (matkul != null) {
                man.remove(matkul);
                man.getTransaction().commit();
                JOptionPane.showMessageDialog(null, "Data berhasil dihapus");
                tampil();
            } else {
                JOptionPane.showMessageDialog(null, "Data tidak ditemukan");
                man.getTransaction().rollback();
            }
        } catch (Exception e) {
            man.getTransaction().rollback();
            JOptionPane.showMessageDialog(null, "Data gagal dihapus: " + e.getMessage());
        } finally {
            man.close();
            manfact.close();
        }
        this.kode_auto();
    } 

### 8. 🖨️ Source code untuk button REPORT

     private void btnReportActionPerformed(java.awt.event.ActionEvent evt) {                                          
        JasperReport reports;

        String reportPath = "C:\\Users\\USER\\Documents\\NetBeansProjects\\UTSPBO\\src\\ReportMK.jasper";
        try {
            try {
                Class.forName(driver);
                conn = DriverManager.getConnection(koneksi, user, password);
            } catch (ClassNotFoundException | SQLException ex) {
                Logger.getLogger(MataKuliah.class.getName()).log(Level.SEVERE, null, ex);
            }
            reports = (JasperReport) JRLoader.loadObjectFromFile(reportPath);
            JasperPrint jprint = JasperFillManager.fillReport(reportPath, null, conn);
            JasperViewer jviewer = new JasperViewer(jprint, false);
            jviewer.setDefaultCloseOperation(DISPOSE_ON_CLOSE);
            jviewer.setVisible(true);
        } catch (JRException ex) {
            Logger.getLogger(MataKuliah.class.getName()).log(Level.SEVERE, null, ex);
        }
    }  

### 9. 🌐 Source code untuk button UPLOAD

     private void btnUpActionPerformed(java.awt.event.ActionEvent evt) {                                      
        JFileChooser jfc = new JFileChooser(FileSystemView.getFileSystemView().getHomeDirectory());
        int returnValue = jfc.showOpenDialog(null);

        if (returnValue == JFileChooser.APPROVE_OPTION) {
            File filePilihan = jfc.getSelectedFile();
            System.out.println("File yang dipilih: " + filePilihan.getAbsolutePath());

            try (BufferedReader buff = new BufferedReader(new FileReader(filePilihan))) {
                String baris;
                String pemisah = ";";

                EntityManagerFactory manfact = Persistence.createEntityManagerFactory("PersistancePU");
                EntityManager man = manfact.createEntityManager();

                man.getTransaction().begin();

                while ((baris = buff.readLine()) != null) {
                    String[] matakuliah = baris.split(pemisah);

                    if (matakuliah.length == 4) {
                        String kodemk = matakuliah[0];
                        String namamk = matakuliah[1];
                        String sks = matakuliah[2];
                        String semesterajar = matakuliah[3];

                        if (!kodemk.isEmpty() && !namamk.isEmpty() && !sks.isEmpty() && !semesterajar.isEmpty()) {
                            try {
                                Matakuliah_1 matkul = new Matakuliah_1();
                                matkul.setKodemk(kodemk);
                                matkul.setNamamk(namamk);
                                matkul.setSks(Integer.parseInt(sks));
                                matkul.setSemesterajar(Integer.parseInt(semesterajar));
                                man.persist(matkul);
                            } catch (Exception e) {
                                JOptionPane.showMessageDialog(null, "Data gagal disimpan: " + e.getMessage());
                            }
                        }
                    }
                }

                man.getTransaction().commit();
                JOptionPane.showMessageDialog(null, "Data berhasil ditambahkan");
                man.close();
                manfact.close();

            } catch (FileNotFoundException ex) {
                JOptionPane.showMessageDialog(null, "File tidak ditemukan: " + ex.getMessage());
            } catch (IOException ex) {
                JOptionPane.showMessageDialog(null, "Error Membaca File: " + ex.getMessage());
            }
        }

        tampil();
    }
