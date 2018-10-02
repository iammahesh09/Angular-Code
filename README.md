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

