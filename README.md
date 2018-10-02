# Angular-Code-Style

List of Modules
---------------
	- NgModule (Main Module)
	- BrowserModule
	- BrowserAnimationsModule
	- HttpClientModule
	- RouterModule
	- FormsModule
	- ReactiveFormsModule
	- BrowserTestingModule
	- CommonModule



List import Paths
-----------------
	# Core:
		- import { 
			NgModule, 
			Injectable, 
			Component,
			OnInit, 
			OnDestroy, 
			Output, 
			Input, 
			Pipe,
			Attribute,
			Directive,
			AfterViewInit, 
			ElementRef,
			Renderer,
			ViewChild
		} from '@angular/core';


	# HTTP:
		- import { HttpClient, HttpHeaders, HttpErrorResponse } from '@angular/common/http';

		- import { CommonModule } from '@angular/common'

	# ROUTER:
		- import { 
			Routes, 
			RouterModule, 
			Router, 
			ActivatedRoute, 
			Params, 
			CanActivate, 
			ActivatedRouteSnapshot, 
			RouterStateSnapshot, 
			CanDeactivate
		} from '@angular/router';

	# Forms:
		- import { FormBuilder, FormGroup, Validators } from '@angular/forms';

	# Rxjs:
		- import { Observable, Subscription, Subject, ErrorObserver, asapScheduler, pipe, of, from, interval, merge, fromEvent } from 'rxjs';
		
		- import { map, first, filter, scan } from 'rxjs/operators';


	# Animations:
		- import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
		- import { trigger, CSSstyle, state, style, transition Timetransition, animate, somekeyframes, generatedquery, stagger } from '@angular/animations';



Routing
-------

	app-routing.module.ts
	---------------------
		import { NgModule } from '@angular/core';
		import { Routes, RouterModule } from '@angular/router';
		import { DepartmentListComponent } from './department-list.component';
		import { EmployeeListComponent } from './employee-list.component';
		import { DepartmentDetailComponent } from './department-detail.component';

		 
		const routes: Routes = [
			{ path: 'departments', component: DepartmentListComponent },
			{ path: 'employees', component: EmployeeListComponent },
			//Passing Parameters
			{ path: 'department/:id', component: DepartmentDetailComponent }
		]

		@NgModule({
			imports: [
				RouterModule.forRoot(routes)
			],
			exports: [
				RouterModule
			]
		})

		export class AppRoutingModule {}

		export const routingComponents = [ 
			DepartmentListComponent, 
			EmployeeListComponent, 
			DepartmentDetailComponent 
		]; 


	app.html
	--------
		<router-outlet></router-outlet>

 		<h3>Department List</h3>
         <ul class="items">
            <li *ngFor="let department of departments" (click)="onSelect(department)">
              {{department.name}}
            </li>
         </ul>


		nav a.router-link-active {
		  color: #039be5;
		}


	app.ts
	------

		import { Router } from '@angular/router';

		departments = [
			{"id": 1, "name": "Angular"},
			{"id": 2, "name": "Node"},
			{"id": 3, "name": "MongoDB"},
			{"id": 4, "name": "Ruby"},
			{"id": 5, "name": "Bootstrap"}
		]
		constructor(private _router: Router) { }

		onSelect(department) {
			this._router.navigate(['/department', department.id]);
		}


		Redirects Routing
		-----------------

		The "redirectTo" property describes the path we want to redirect this user to if they navigate to this URL.

			const routes:Routes = [
				{path: '', redirectTo: 'home', pathMatch: 'full'}, 
				{path: 'find', redirectTo: 'search'}, 
				{path: 'home', component: HomeComponent},
				{path: 'search', component: SearchComponent}
			];


			# Note:

				For the special case of an empty URL we also need to add the " pathMatch: 'full' " property so Angular knows it should be matching exactly the empty string and not partially the empty string.


			We’ve also added a redirect from /find to /search, since this isn’t empty we don’t need to add the pathMatch property.



Receiving Parameters (params)
-----------------------------

	<h3>You selected department with id = {{departmentId}}</h3>


	import { Router, ActivatedRoute } from '@angular/router';

  	
  	public departmentId;

	constructor(private _activatedRoute: ActivatedRoute){}
	
	ngOnInit() {
		let id = parseInt(this._activatedRoute.snapshot.params['id']);
		this.departmentId = id;
	}



Params Observable
-----------------
	import { Router, ActivatedRoute, Params } from '@angular/router';

	@Component({  
	  template: `
	  		<h3>You selected department with id = {{departmentId}}</h3>
	        <a (click)="goPrevious()">Previous</a>
	        <a (click)="goNext()">Next</a>
	        <a (click)="gotoDepartments()">Back</a>
	  	`
	})



	constructor(private _router: Router){}


	ngOnInit() {
		this._router.params.subscribe(
			(_params: Params) => {
				let id = parseInt(_params['id']); 
				this.departmentId = id;
			});
	}

	goPrevious(){
		let previousId = this.departmentId - 1;
		this._router.navigate(['/department', previousId]);
	}

	goNext(){
		console.log('Clicked next');
		let nextId = this.departmentId + 1;    
		this._router.navigate(['/department', nextId]);
	}

	
	gotoDepartments(){
		//Optional Params
	    let selectedId = this.departmentId ? this.departmentId : null;
	    this._router.navigate(['/departments', {id: selectedId}]);
  	}



Angular 6 Alert Component (Subscription)
-------------------------

	<div *ngIf="status" [ngClass]="{ 'alert': status, 'alert-success': status.type === 'success', 'alert-danger': status.type === 'error' }">{{status.text}}</div>


	alert.component.ts
	------------------
		import { Component, OnInit, OnDestroy } from '@angular/core';
		import { Subscription } from 'rxjs';
		 
		import { AlertService } from '../_services';
		 
		@Component({
		    selector: 'alert',
		    templateUrl: 'alert.component.html'
		})
		 
		export class AlertComponent implements OnInit, OnDestroy {
		    private subscription: Subscription;
		    status: any;
		 
		    constructor(private alertService: AlertService) { }
		 
		    ngOnInit() {
		        this.subscription = this.alertService.getMessage().subscribe(status => { 
		            this.status = status; 
		        });
		    }
		 
		    ngOnDestroy() {
		        this.subscription.unsubscribe();
		    }
		}



App.module.ts
-------------

	import { BrowserModule } from '@angular/platform-browser';
	import { NgModule } from '@angular/core';

	import { AppRoutingModule } from './app-routing.module';
	import { AppComponent } from './app.component';

	@NgModule({
	  declarations: [
	    AppComponent
	  ],
	  imports: [
	  	BrowserModule,
	    AppRoutingModule
	  ],
	  providers: [],
	  bootstrap: [AppComponent]
	})
	export class AppModule { }


Angular 6 Animation
-------------------

	> npm install @angular/animations@latest --save


	app.module.ts
	-------------
		import { BrowserAnimationsModule } from '@angular/platform-browser/animations';

		@NgModule({
		  ...
		  imports: [
		    BrowserAnimationsModule
		  ],
		})


	Angular 6 Animation Syntx:
	--------------------------

	import { trigger, CSSstyle, Timetransition, animate, somekeyframes, generatedquery, stagger } from '@angular/animations';

	@Component({

		selector: 'app-clients',

		template: '<ul [@packageEmployes]="clients$">',

		styleUrls: ['./clients.component.scss'],

		animations: [

			trigger('packageEmployes', [
			  	transition('* <=> *', [
			    	query(':enter',
			      		[
			        		style({ opacity: 0, transform: 'translateY(-15px)' }),
			        		stagger(
			          			'60ms',
			         			animate(
				            		'560ms ease-out',
				           			style({ opacity: 1, transform: 'translateY(0px)' })
			          			)
			        		)
			      		],
			      	{ optional: true }
			    ),

			    query(':leave', animate('60ms', style({ opacity: 0 })), {
			      optional: true
			    })

			  ])
			])
		]
	})



Angular 6 Http Request Methods
------------------------------

	import { HttpHeaders } from '@angular/common/http';

	const httpOptions = {
		headers: new HttpHeaders({
			'Content-Type':  'application/json',
			'Authorization': 'Your-angular6-Live-auth-token'
		})
	};

	 // POST: httprequest - Http post and get request in angular 6
		addusername (user: username): Observable<username> {
			return this.http.post<username>(this.useresUrl, user, httpOptions).pipe(
		      catchError(this.handleError('addusername', user))
		    );
		}


	// POST: Angular 6 Http Post Method with Parameters Example
		addUser (user: User): Observable<User> {
			return this.http.post<User>(this.useresUrl, user, httpOptions).pipe(
				catchError(this.handleError('addUser', user))
			);
		}
	// GET Angular 6 Http GET Method with Parameters Example
		getUsers (): Observable<User[]> {
			return this.http.get<User[]>(this.useresUrl).pipe(
				catchError(this.handleError('getUsers', []))
			);
		}



Angular 6 Auth & Authentication Service (Create Token and get Token)
---------------------------------------

	auth.guard.ts
	-------------
		import { Injectable } from '@angular/core';
		import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, CanDeactivate, Router } from '@angular/router';
		import { Observable } from 'rxjs';
		import { AuthService } from './auth.service';

		@Injectable({
			providedIn: 'root'
		})
		export class AuthGuard implements CanActivate {

		constructor(private _authService: AuthService, private _router: Router) { }

			canActivate( next: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {
			
				let url: string = state.url;
				console.log("Satte Url = " + url)
				
				if (this._authService.isAuthenticated()) {
					return true;
				} else {
					this._router.navigate(['/sign-in']);
					return false
				}

			}
		}



	auth.service.ts
	---------------
		import { Injectable } from '@angular/core';
		import { HttpClient, HttpErrorResponse } from '@angular/common/http';
		import { User } from '../models/user.model';
		import { Router } from '@angular/router';
		import { Subject, throwError } from 'rxjs';
		import { map, catchError } from 'rxjs/operators';

		@Injectable({
			providedIn: 'root'
		})
		
		export class AuthService {

			loginUrl: string = "http://localhost:9001/api/users/login";

			constructor(private _http: HttpClient, private _router: Router) { }

			login(userData: User) {
				return this._http.post(this.loginUrl, userData);
			}

			saveToken(token: string) {
				return sessionStorage.setItem("token", token)
			}

			getToken() {
				return sessionStorage.getItem('token')
			}

			isLoggedin() {
				return !!sessionStorage.getItem("token");
			}

			logout() {
				localStorage.removeItem("token");
				this._router.navigate(['/sign-in'])
			}

			isAuthenticated(): boolean {
				const GetToken = sessionStorage.getItem('token');
				
				if (GetToken && GetToken.length > 0) {
					return true;
				} else {
					return false;
				}
			}

		}


	sign-in.component.ts
	--------------------	
		import { Component, OnInit } from '@angular/core';
		import { AuthService } from '../services/auth.service';
		import { Router } from '@angular/router';

		@Component({
			selector: 'app-sign-in',
			templateUrl: './sign-in.component.html',
			styleUrls: ['./sign-in.component.css']
		})

		export class SignInComponent implements OnInit {

			_user: any = {}

			loginResult: boolean;
			loginResultMsg: string;
			loginError: boolean = false;
			constructor(private _authService: AuthService, private _router: Router) { }

			userLogin() {

				this._authService.login(this._user).subscribe(
					res => {
						this.loginResult = true;
						this.loginError = true;
						this.loginResultMsg = "Successfully login"
						this._authService.saveToken(res["token"])
						this._router.navigate(["/dashborad"]);
					},
					error => {
						this.loginResult = true;
						this.loginError = true;
						this.loginResultMsg = "Error! Please check username and password details"
						this._router.navigate(['/sign-in'])
					}
				)
			}

			ngOnInit() {
			}

		}



	sign-in.component.html
	----------------------

		<div *ngIf="loginError" [class]="loginResult?['alert alert-danger']:['alert alert-success']">{{loginResultMsg}}</div>
		
		<form novalidate #loginForm="ngForm">
			
			<div class="form-group">
				<input type="email" placeholder="Enter email" [(ngModel)]="_user.username" #username="ngModel" name="username">		
			</div>
			
			<div class="form-group">
				<input type="password" placeholder="Password" [(ngModel)]="_user.password" #password="ngModel" name="password">
			</div>
			
			<button type="submit" class="btn btn-success" [disabled]="loginForm.invalid" (click)="userLogin()">Submit</button>

		</form>


	
	product.service.ts (get Token)
	------------------

		import { Injectable } from '@angular/core';
		import { HttpClient } from '@angular/common/http';
		import { AuthService } from './auth.service';
		import { Product } from "../models/product.model";

		@Injectable({
			providedIn: 'root'
		})
		
		export class productService {

			constructor(private _http: HttpClient, private _authService: AuthService) { }

			getProductData() {

				var hder = { 'authorization': this._authService.getToken() };

				return this._http.get<Product[]>("http://localhost:9001/api/products/", { headers: hder })
			}

			saveProduct(data) {

				var hder = { 'authorization': this._authService.getToken() };

				return this._http.post<Product[]>("http://localhost:9001/api/products/", data, { headers: hder })
			}


		}



Angular 6 Service (Student)
-------------------------
	import { Injectable } from '@angular/core';
	import { HttpClient } from '@angular/common/http';
	 
	import { Student } from '../_models';
	 
	@Injectable({
	  providedIn: 'root'
	})

	export class StudentService {
	    constructor(private http: HttpClient) { }
	 
	    getAll() {
	        return this.http.get('/api/students');
	    }
	 
	    getById(id: number) {
	        return this.http.get('/api/students/' + id);
	    }
	 
	    create(student: Student) {
	        return this.http.post('/api/students', student);
	    }
	 
	    update(student: Student) {
	        return this.http.put('/api/students/' + student.id, student);
	    }
	 
	    delete(id: number) {
	        return this.http.delete('/api/students/' + id);
	    }
	}



Angular Basic Form Validation
-----------------------------

	basic-form.component.html
	-------------------------
		<h2 class="text-center mt-5">Angular 6 Basic Form Validation</h2>

		<div class="jumbotron">
		  <div class="col-sm-8 offset-sm-2">

		    <form novalidate #basicForm="ngForm">
		      <div class="form-group">
		        <label>First Name</label>
		        <input type="text" class="form-control" required minlength="4" name="firstName" [(ngModel)]="_user.firstName"
		          #firstName="ngModel" placeholder="First Name">

		        <div *ngIf="firstName.invalid && (firstName.dirty || firstName.touched)" class="text-danger">
		          <div *ngIf="firstName.errors.required">First Name is required.</div>
		          <div *ngIf="firstName.errors.minlength">Name must be at least 4 characters long.</div>
		        </div>
		      </div>

		      <div class="form-group">
		        <label>Last Name</label>
		        <input type="text" class="form-control" required [(ngModel)]="_user.lastName" #lastName="ngModel" name="lastName"
		          placeholder="Last Name" />
		        <div *ngIf="lastName.invalid && (lastName.dirty || lastName.touched)" class="text-danger">
		          <div *ngIf="lastName.errors.required">Last Name is required.</div>
		        </div>
		      </div>

		      <div class="form-group">
		        <label>Email</label>
		        <input type="text" class="form-control" required [(ngModel)]="_user.email" #email="ngModel" name="email"
		          placeholder="Email" />

		        <div *ngIf="email.invalid && (email.dirty || email.touched)" class="text-danger">
		          <div *ngIf="email.errors.required">Email is required.</div>
		        </div>
		      </div>

		      <div class="form-group">
		        <label>Password</label>
		        <input type="password" class="form-control" minlength="6" required [(ngModel)]="_user.password" #password="ngModel"
		          name="password" placeholder="password" />

		        <div *ngIf="password.invalid && (password.dirty || password.touched)" class="text-danger">
		          <div *ngIf="password.errors.required">Password is required</div>
		          <div *ngIf="password.errors.minlength">Password must be at least 6 characters</div>
		        </div>

		      </div>
		      <button type="submit" [disabled]="basicForm.invalid" (click)="onSubmit()" class="btn btn-primary">Register</button>

		    </form>

		  </div>
		</div>



Angular Form validation (FormBuilder, FormGroup, Validators)
-------------------------

	import { Component, OnInit } from '@angular/core';
	import { Router, ActivatedRoute } from '@angular/router';
	import { FormBuilder, FormGroup, Validators } from '@angular/forms';
	import { first } from 'rxjs/operators';
	 
	import { AlertService, AuthenticationService } from '../_services';
	 
	@Component({templateUrl: 'login.component.html'})

	export class LoginComponent implements OnInit {

	    signInFrm: FormGroup;
	    Processing = false;
	    frmSubmit = false;
	    callbackUri: string;
	 
	    constructor(
	        private formBuilder: FormBuilder,
	        private route: ActivatedRoute,
	        private router: Router,
	        private authenticationService: AuthenticationService,
	        private alertService: AlertService) {}
	 
	    ngOnInit() {
	        this.signInFrm = this.formBuilder.group({
	            studentname: ['', Validators.required],
	            password: ['', Validators.required]
	        });
	 
	        this.authenticationService.logout();
	 
	        this.callbackUri = this.route.snapshot.queryParams['callbackUri'] || '/';
	    }
	 
	    get f() { return this.signInFrm.controls; }
	 
	    onSubmit() {
	        this.frmSubmit = true;
	        if (this.signInFrm.invalid) {
	            return;
	        }
	 
	        this.Processing = true;
	        this.authenticationService.login(this.f.studentname.value, this.f.password.value)
	            .pipe(first())
	            .subscribe(
	                data => {
	                    this.router.navigate([this.callbackUri]);
	                },
	                error => {
	                    this.alertService.error(error);
	                    this.Processing = false;
	                });
	    }
	}



ReactiveForms Validation At The Angular
---------------------------------------

	HTML
	----
		<form [formGroup]="memberFrm" novalidate>

			<div class="form-group">
				<label class="col-md-4">Member Name</label>
				<input type="text" class="form-control" formControlName="name" #name />
			</div>

			<div *ngIf="memberFrm.controls['name'].invalid && (memberFrm.controls['name'].dirty || memberFrm.controls['name'].touched)" class="alert alert-danger">
				<p *ngIf="memberFrm.controls['name'].errors.required">Name is required.</p>
			</div>

			<div class="form-group">
				<label class="col-md-4">Member Age</label>
				<input type="text" class="form-control" formControlName="age" #age/>
			</div>

			<div *ngIf="memberFrm.controls['age'].invalid && (memberFrm.controls['age'].dirty || memberFrm.controls['age'].touched)" class="alert alert-danger">
				<p *ngIf="memberFrm.controls['age'].errors.required">Age is required.</p>
			</div>

			<div class="form-group">
				<button (click)="addMember(name.value, age.value)" [disabled]="memberFrm.pristine || memberFrm.invalid" class="btn btn-success">Add</button>
			</div>
		</form>


	Create.component.ts
	-------------------

		import { Component, OnInit } from '@angular/core';
		import { FormGroup,  FormBuilder,  Validators } from '@angular/forms';
		import { MemberService } from '../../member.service';

		@Component({
			selector: 'app-create',
			templateUrl: './create.component.html',
			styleUrls: ['./create.component.css']
		})
		
		export class CreateComponent implements OnInit {

				title = 'Add Member';
				memberFrm: FormGroup;

			constructor(private memberservice: MemberService, private fb: FormBuilder) {
				this.createForm();
			}

			createForm() {
				this.memberFrm = this.fb.group({
					name: ['', Validators.required ],
					age: ['', Validators.required ]
				});
			}

			addMember(name, age) {
				this.memberservice.addMember(name, age);
			}

			ngOnInit(){}

		}

