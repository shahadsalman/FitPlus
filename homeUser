import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:fitplus/firebase/fb_auth_controller.dart';
import 'package:fitplus/firebase/fb_firestore.dart';
import 'package:fitplus/screens/details_gym/details_gym_screen.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';
//import 'package:gap/gap.dart';

class HomeScreenUser extends StatelessWidget {
  const HomeScreenUser({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) => Scaffold(
        appBar: AppBar(
          backgroundColor:Color.fromARGB(255, 210, 199, 226),
          elevation: 20,
          centerTitle: true,
          actions: <Widget>[
            IconButton(
              onPressed: () {
                //notafication
              },
              icon: const Icon(Icons.notifications),
            ),
          ],
          bottom: PreferredSize(
            preferredSize: const Size(10, 60),
            child: Column(children: <Widget>[
              Padding(
                  padding: const EdgeInsets.all(10),
                  child: Container(
                    height: 45,
                    decoration: BoxDecoration(
                      borderRadius: BorderRadius.circular(10),
                      color: Colors.white,
                    ),
                    child: TextFormField(
                      decoration: const InputDecoration(
                          prefixIcon: Icon(
                            Icons.search,color: Color.fromARGB(255, 16, 16, 16)
                          ),
                          hintText: "Search",
                          hintStyle: TextStyle(color: Color.fromARGB(255, 16, 16, 16)),
                          helperStyle: TextStyle(fontSize: 50)),
                    ),
                  ))
            ]),
          ),
        ),
        drawer: const NavigationDrawer(),
        body: StreamBuilder<QuerySnapshot>(
          stream: FbFireStoreController().read(
              nameCollection: 'users',
              descending: true,
              orderBy: 'typeAccount'),
          builder: (context, snapshot) {
            if (snapshot.connectionState == ConnectionState.waiting) {
              return Center(child: CircularProgressIndicator());
            } else if (snapshot.hasData && snapshot.data!.docs.isNotEmpty) {
              List<QueryDocumentSnapshot> data = snapshot.data!.docs;
              List<QueryDocumentSnapshot> listGyme = [];
              for (var gym in data) {
                if (gym.get('typeAccount') == 2) {
                  listGyme.add(gym);
                }
              }
              return ListView.separated(
                  itemBuilder: (context, index) {
                    return GestureDetector(
                      onTap: () {
                        Get.to(()=>DetailsGymScreen(data: listGyme[index],));
                      },
                      child: Container(
                        height: 100,
                        width: double.infinity,
                        margin: EdgeInsetsDirectional.only(top: 10),
                        child: Row(
                          children: [
                            SizedBox(
                              width: 10,
                            ),
                            Container(
                              height: 90,
                              width: 90,
                              decoration: BoxDecoration(
                                border: Border.all(color: Colors.black),
                              ),
                              child: Image.network(
                                listGyme[index]['logo'],
                                height: 100,
                                fit: BoxFit.fill,
                                loadingBuilder:
                                    (context, child, loadingProgress) {
                                  if (loadingProgress == null) return child;
                                  return Center(
                                      child: CircularProgressIndicator());
                                },
                              ),
                            ),
                            SizedBox(
                              width: 10,
                            ),
                            Expanded(
                              child: Column(
                                mainAxisAlignment: MainAxisAlignment.center,
                                children: [
                                  Row(
                                    children: [
                                      Text(
                                        listGyme[index]['nameGym'],
                                        style: TextStyle(
                                            fontWeight: FontWeight.bold,
                                            fontSize: 16),
                                      ),
                                    ],
                                  ),
                                  Flexible(
                                    child: Container(width: double.infinity,
                                    child: Text(
                                      listGyme[index]['descriptionGym'],
                                      maxLines: 2,
                                      textAlign: TextAlign.start,
                                      style: TextStyle(
                                        fontWeight: FontWeight.normal,
                                        fontSize: 13,
                                      ),
                                    ),)
                                  ),
                                  SizedBox(
                                    height: 10,
                                  ),
                                  Row(
                                    children: [
                                      Icon(
                                        Icons.location_on_outlined,
                                        size: 14,
                                      ),
                                      Flexible(
                                        child: Text(
                                          listGyme[index]['locationGym'],
                                          maxLines: 1,
                                          style: TextStyle(
                                            fontWeight: FontWeight.normal,
                                            fontSize: 13,
                                          ),
                                        ),
                                      ),
                                    ],
                                  )
                                ],
                              ),
                            )
                          ],
                        ),
                      ),
                    );
                  },
                  separatorBuilder: (context, index) {
                    return Container(
                      height: 2,
                      color: Color(0xff48358e),
                    );
                  },
                  itemCount: listGyme.length);
            } else {
              return Center(
                child: Column(
                  mainAxisSize: MainAxisSize.min,
                  children: [
                    Icon(Icons.warning, size: 85, color: Colors.grey.shade500),
                    Text(
                      'Empty',
                      style: TextStyle(
                        color: Colors.grey.shade500,
                        fontSize: 18,
                        fontWeight: FontWeight.bold,
                        fontFamily: 'TimesNewRoman',
                      ),
                    )
                  ],
                ),
              );
            }
          },
        ),
      );
}

class NavigationDrawer extends StatelessWidget {
  const NavigationDrawer({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) => Drawer(
        child: SingleChildScrollView(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: <Widget>[
              buildHeader(context),
              builMenuItems(context),
            ],
          ),
        ),
      );

  Widget buildHeader(BuildContext context) => Container(
        padding: EdgeInsets.only(
          top: MediaQuery.of(context).padding.top,
        ),
      );

  Widget builMenuItems(BuildContext context) => Column(
        children: [
          ListTile(
            leading: const Icon(Icons.supervised_user_circle,color:Color.fromARGB(255, 0, 0, 0) ,),
            title: const Text('Profile',),textColor: Color.fromARGB(255, 0, 0, 0),
            onTap: () {},
          ),
          ListTile(
            leading: const Icon(Icons.logout,color:Color.fromARGB(255, 0, 0, 0),),
            title: const Text('Logout'),textColor: Color.fromARGB(255, 0, 0, 0),
            onTap: () {
              FbAuthController().signOut();
            },
          ),
        ],
      );
}
