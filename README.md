1. 다운로드
 - 링크 제공
1. 설치하기
 - 간단한 설치 도움말 제공
 - 링크로 스크린 샷 설치방법 제공 (jar 파일 복사 붙여 넣기 후, 빌드 패스 설정까지)
1. 샘플프로젝트
 - CRUD에 해당하는 간단 예제 및 Line Of Code 비교
 - create <p><pre><code>DatabaseOpenHelper databaseHelper = new DatabaseOpenHelper(this);
EntityManager em = EntityManagerFactory.getEntityManager();
entityManager.createTable(My.class, SQLiteDatabaseUtils.getWritableDatabase(databaseHelper));
</code></pre></p>
 - insert/update <p><pre><code>EntityManager em = EntityManagerFactory.getEntityManager();
em.getTransaction().begin();
MyClass my = new MyClass(“something”);
em.persist(my);
em.getTransaction().commit();
</code></pre></p>
 - find <p><pre><code>EntityManager em = EntityManagerFactory.getEntityManager();
em.getTransaction().begin();
MyClass my = em.find(My.class, "something");
em.getTransaction().commit();
</code></pre></p>
 - delete <p><pre><code>DatabaseOpenHelper databaseHelper = new DatabaseOpenHelper(this);
EntityManager em = EntityManagerFactory.getEntityManager();
entityManager.dropTable(My.class, SQLiteDatabaseUtils.getWritableDatabase(databaseHelper));
</code></pre></p>
1. Cookbook 링크
1. Troubleshooting 링크
1. JPA 공부방 링크
